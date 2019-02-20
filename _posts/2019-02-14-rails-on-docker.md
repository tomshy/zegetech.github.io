---
layout: blog
title: Rails on Docker
date: 2019-02-14 08:00 +0300
categories: developer
author: Melvin Atieno, Ngari Ndung'u, Tom Nyongesa, Kariuki Gathitu
blog-image: rails-docker/railsondocker.png
intro: Rails and Docker are important components in the development processes at Zegetech. Rails is our chosen platform for most of what we build, and docker provides pain-free environment management both for development and in production. We have previously covered these two technologies separately, and this post covers the sweet spot at their intersection. We will take you through the process of configuring a rails development environment on docker, and configure a postgresql database for it.
---
![Cover Image](/assets/images/blog/{{page.blog-image}}){:class="img-responsive center"}

{{page.intro}}

You might want to take a look at our previous blogs for some background information on what we will be setting up. 
- [What and Why Ruby on Rails](2018-10-17-why-ruby-on-rails.md)
- [What and Why Docker](2018-10-29-what-and-why-docker.md)
- [Developing with Docker](2018-11-08-developing-with-docker.md)

## Our goal
We want to build an app and have chosen Ruby on Rails as our framework. But because docker is awesome and a key component in developing and keeping things clean, we want to set up a development environment that allows us to work on a Rails application that resides in a docker container on our local machine. At the end of it, well have
1. A rails app with a postgresql Database
2. No local ruby, rails or postgresql dependancies on our machine. Everything will be dockerized.

## Pre-requisites
1. The command `docker -v` should work
2. The command `docker-compose -v` should work
3. The command `git --version` should work
3. Refer to the [installation docs](https://docs.docker.com/install/linux/docker-ce/ubuntu/#install-docker-ce) to upgrade if neccesary.

## The Rails Docker image
In order to start developing, we need a docker container with the rails environment. We'll build an image for that. Docker keeps track of your image as you make edits in the docker cache. In order to track, we need the main files that manage the rails application as well as the main docker files. First thing you need according to [docker compose](https://docs.docker.com/compose/rails/#define-the-project) are the following files
1. `Dockerfile` using whatever is the latest version of ruby on alpine. We use alpine because we want the smallest footprint possible for out app. If you change to another distro like ubuntu, then make sure you use the appropriate package manager e.g. apt-get instead of apk
2. `Gemfile` with rails declaration
3. `Gemfile.lock` that will be blank
4. `docker-compose` to make running docker commands easier
5. `.dockerignore` file to exclude certain files from the build. This helps keep the image small.

These four files need to reside in the folder where our app will be. Details of what the commands do are included in the comments alongside the commands.

```docker
# Dockerfile
FROM ruby:2.5.1-alpine
LABEL maintainer="Kariuki Gathitu <kgathi2@gmail.com>"
LABEL version="1.0"

# Packages needed to get Rails running in Alpine.
# DB_PACKAGES="sqlite-dev postgresql-dev mysql-dev" \
ENV BUILD_PACKAGES="curl-dev ruby-dev build-base bash" \
    DEV_PACKAGES="zlib-dev libxml2-dev libxslt-dev tzdata yaml-dev" \
    DB_PACKAGES="postgresql-dev postgresql-client" \
    RUBY_PACKAGES="ruby-json yaml nodejs"

# Update and install base packages 
RUN apk update && \
    apk upgrade && \
    apk add --update\
    $BUILD_PACKAGES \
    $DEV_PACKAGES \
    $DB_PACKAGES \
    $RUBY_PACKAGES && \
    rm -rf /var/cache/apk/* && \
    mkdir -p /usr/src/app

# Create system user to run as non-root. 
RUN addgroup -S admin -g 1000 && adduser -S -g '' -u 1000 -G admin deploy

# Set the Rails Environment Variables for production
ENV RAILS_ROOT /home/deploy/app
ENV RAILS_LOG_TO_STDOUT 1
ENV RAILS_ENV production

# Set user as deploy from here on out
USER deploy

# Configure the main working directory. This is the base
# directory used in any further RUN, COPY, and ENTRYPOINT
# commands.
RUN mkdir -p $RAILS_ROOT
WORKDIR $RAILS_ROOT

# Copy the Gemfile as well as the Gemfile.lock and install
# the RubyGems. This is a separate step so the dependencies
# will be cached unless changes to one of those two files
# are made.
COPY --chown=deploy:admin Gemfile Gemfile.lock ./
RUN gem install bundler
RUN bundle install --jobs 20 --retry 5

# Copy the main application.
COPY --chown=deploy:admin . ./

# Expose the applications port to the host machine
EXPOSE 3000

# Command to run when the container is started. 
CMD ["puma", "-C", "config/puma.rb"]
```

{:.image-attribution}
Dockerfile

We run the Dockerfile installation commands as a new `deploy` user, which is the [recommended practice](https://medium.com/@mode/processes-in-containers-should-not-run-as-root-2feae3f0df3b). And when we copy files across, in order to avoid permission issues, we make sure the copy user is the non-root user we created. 

```ruby
# Gemfile
source 'https://rubygems.org'
gem 'rails'
```

{:.image-attribution}
Gemfile

```yaml
# docker-compose.yml
version: '3'
services:
  app:
    build: .
    command: 'puma -C config/puma.rb'
    stdin_open: true
    tty: true
    environment:
      - RAILS_ENV=development
    ports:
      - '80:3000'
    links:
      - postgres
    volumes:
      - .:/home/deploy/app

  postgres:
    image: postgres:9.6.2-alpine
    environment:
      - POSTGRES_PASSWORD=mysecretpassword
    ports:
      - '5432'
    volumes:
      - data:/var/lib/postgresql/data

volumes:
  data:
```

{:.image-attribution}
docker-compose.yml

Mapping volumes in the app service, `.:/home/deploy/app` only happens when running with docker compose. This is so that the developer can have realtime interaction with the app in the container when developing. Otherwise, you'd need to restart the container everytime you made changes. This however does not extend to the initial files we had, mainly the `Gemfile`. Any Change to the `Gemfile` or `Dockerfile` will require a rebuild of the container. We have also used a different service for the database. Postgres will reside in its own container. We are also binding the container port `3000` to our host machines (your laptop) port `80`.

```bash
# .dockerignore
**/.git*
**/*.sqlite3
**/*.sqlite3-journal
log/*
tmp/*
storage/*
**/README.md

secrets/*
**/docker-compose.yml
**/Dockerfile
```

{:.image-attribution}
.dockerignore

## Rails App
### 1. Generate New Rails app
Now that we have our configuration done, all that is left is to generate the rails app using docker compose

```
docker-compose run --no-deps app rails new . --force --database=postgresql

# For API only app
docker-compose run --no-deps app rails new . --force --database=postgresql --api

#then rebuild image due to new Gemfile
docker-compose build
```
This generates a new rails boilerplate in the current directory. Because our volumes are mapped, anything happening in the container is reflected on the host machine, so our directory will now have a brand new rails application. The `--no-deps` flag tells compose not to start dependent services, in this case the *postgres* service.

### 2. Connect the database
We have the postgres DB in its own container with the official image from docker hub. We need to point our app to this database instance. 

```yaml
# app/config/database.yml

default: &default
  adapter: postgresql
  encoding: unicode
  pool: 5
  # Database credentials
  host: postgres # name for db service in docker-compose.yml
  username: postgres # default user for postgresql docker image
  password: mysecretpassword # must match POSTGRES_PASSWORD in docker-compose.yml
```

{:.image-attribution}
app/config/database.yml

Now start the app daemonized

```bash
docker-compose up -d
```
The app will start in development mode because the docker-compose.yml file overrides the env `RAILS_ENV`. In another window, initialize the DB.
```bash
docker-compose exec app rails db:create
# or if the container was not yet started
docker-compose run app rails db:create
```
The database file are persisted in the `data:` docker volume. Without it you would need to run `docker-compose run web rake db:create` whenever restarting your app to recreate the database.

Your app should be available at [localhost](http://localhost). 
![Rails welcome](/assets/images/blog/rails-docker/rails_welcome.png){:class="img-responsive center"}
To stop the application run `docker-compose down`.
If all went well, then we need to "save" our app in git. Stop the app and check it into git
```bash
docker-compose down
git add .
git commit -m 'Version 0.0.0 -  App Initialized'
```
So now we have our app boilerplate ready with persistence on a separate postgresql database. 

### 3. Code
As beautiful as the rails welcome page is, it doesn't tell us if our environment behaves as we need it to. Let's do some quick scaffolding to test.
First bring the app back up with `docker-compose up`, then:
```shell
docker-compose exec app bundle exec rails g scaffold user username first_name last_name phone:integer
```
Then run the migrations:
```shell
docker-compose exec app bundle exec rails db:migrate
```
And edit our routes to point to our list of users:
```ruby
Rails.application.routes.draw do
  resources :users
  root "users#index"
  # For details on the DSL available within this file, see http://guides.rubyonrails.org/routing.html
end
```
Reload the page and voila:
![User index](/assets/images/blog/rails-docker/user_index.png){:.img-responsive .center}
You can add a few users to populate the page.
![Sample users](/assets/images/blog/rails-docker/user_list.png){:.img-responsive .center}
Finally, lets shutdown our server and see if it comes back up with what we expect.
Hit `Ctrl-C` to bring down the services and wait for them to exit. Restart with a `docker-compose up`.
If everything went well you should be greeted with the list of users you created. And ..., breathe!

### Avoid If You Can
1. Building a custom image - this is all well and good for the learning experience or when there isn't a ready image on dockerhub. Otherwise, it's a pain not worth having.
2. Installing postgres in the rails container - two reasons why; using a separate database image means you can re-use it in a new app, and second, making postgres work in the app container is a major hustle. You'll need to set up the postgres user, set up authentication and find a way to make sure the db service is started when you run your app.
3. Bind mounts for 3rd-party application data - use docker managed volumes for data you don't need to interact with directly. Docker sets up the volumes so that the container has the proper access rights, helping avoid a world of pain in managing file permissions.

And that's that... finally. We have our environment set up and can finally bring that app to life!
Whatever you choose to build, *ganbatte*.

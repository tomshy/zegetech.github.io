---
layout: blog
title: Localhost tunneling
date: 2019-03-07 12:09 +0300
categories: developer
author: Tom Nyongesa
blog-image: localhost-tunneling/tunneling.jpg
intro: Sometimes when i work on a project on my laptop, I need to show my boss, workmates, client or just want to test it on my phone. I can do this by taking my development environments(my laptop) to them but what if I'm working on a desktop? Well, I can ask them to come over or ask them to join my network, what if they are thousands of miles away? Well, I can deploy it to the internet but what if I'm not just ready? Well, say hello to [**Localhost Tunneling**](https://en.wikipedia.org/wiki/Tunneling_protocol), my bridge to the outside world! 
---

![Localhost tunneling](/assets/images/blog/{{page.blog-image}}){:.img-responsive .center}

{{page.intro}}

It is simply a tunnel from an external server (open to the internet) to my local server through the artifacts that separate the two worlds such as NATs and firewalls!
Here is a pictorial representation of how it works.
![start](/assets/images/blog/localhost-tunneling/local-tunnel.png){:.img-responsive .center}

Some of the most commonly used localhost tunneling tools among developers include:

## [Ngrok](https://ngrok.com/)

This is arguably the most popular localhost tunneling tool around. It can be installed via a package manager like npm or [downloaded](https://dashboard.ngrok.com/get-started) then unzipped to your machine.

Via npm,

```bash
npm install -g ngrok
```

Upon successful installation, fire it up by running:

```bash
ngrok http 80
```

This will open another bash window with the url forwarding info. Click on the listed urls and you should be able to access your local server publicly. So now, whatever is running on `http://localhost` (in this case port 80) will be accesible on a unique ngrok link say `http://92832de0.ngrok.io`

Ngrok also provides other advanced features like password protection and creation of custom sub-domains that are reserved for premium users. Check them out on [their website](https://ngrok.com/).

## [Localtunnel](https://localtunnel.me/)
Another exciting easy to use local tunneling tool. To install, run the following

```bash
npm install -g localtunnel
```

If you experience permission issue, install as admin, though not recommended. You can also try installing it with 'yarn'

Start your localserver if you aren't already running one then,

```bash
lt --port 80 
```

## [Burrow](https://burrow.io/)

Not interested in installation headaches and package management? Burrow got your back! Burrow is another tunneling service that makes the whole tunneling process simpler. You simply need to sign up on their platform, confirm your email and create a tunnel. Works like a charm!

However, you are required to upgrade your membership @ $4.99/month to continue using their service for another day.

## [PageKite](https://pagekite.net/)
Here is another amazing local tunneling tool that has been around for a while and popular among the minecraft gamers. To install, you need to ensure that you have python installed on your machine. Then head to [Pagekite downloads](http://pagekite.net/downloads) page to get the appropriate install for your machine. 

Here is a quick start for linux users:

```bash
curl -O https://pagekite.net/pk/pagekite.py
```

Then,

```bash
python2 pagekite.py 80 yourkitename.pagekite.me
```

One amazing thing with Pagekite is that you can serve your static files without having a local server on your machine. Cool, right? 

Simply include the path of your static site to the command above,

```bash
python2 pagekite.py path/to/static-file yourkitename.pagekite.me
```

## Other local tunneling tools

- [Serveo](https://serveo.net/)
- [Forward](https://forwardhq.com/)
- [Proxy Local](https://github.com/proxylocal/proxylocal-gem)

The whole tunnelling thing is really simple and easy to get started with. So, if you are working on something and you would like others to see what you are building, pick a tool and fire it up!

![start](/assets/images/blog/localhost-tunneling/ignite.webp){:.img-responsive .center}
---
layout: blog
title: Journey from PHP to Ruby
date: 2019-04-25 17:43 +0300
categories: developer
published: true
author: Tom Nyongesa
blog-image: php-ruby/phpruby.png
intro: Learning anything new has always been an uphill task. A good example is trying to learn a new language like Spanish if you are a Swahili speaking native. I don't know Spanish so I won't say much about it but what i gathered is that communication in spanish is 98% similar to that in Swahili with little differences in things like gender specificity. The huge difference comes in how they structure the wordings. Same thing to programming languages. 
---

![phptoruby](/assets/images/blog/{{page.blog-image}}){:.img-responsive .center}
{{page.intro}}

All programming languages comply to the [Turing Completeness phenomenon](http://wiki.c2.com/?TuringComplete). So, technically anything done in language A can be done in language Z.

## My experience

The focus of this piece isn't really confined to PHP developers moving to ruby but rather the switching between any two languages. I'm just putting the piece into context from what I have gone through in my transition.

PHP has been around for decades making it the tools of choice for many new developers to get into the world of web development because of its huge and well developed ecosystem. It's normally the first language newbies are introduced to.

My primary development language has been PHP with Laravel being the main framework of development until luckily, I was introduced to [Ruby](https://www.ruby-lang.org/) by our CEO Kariuku Gathitu 3 months ago. Lucky because i though PHP was "be all and end all" of programming. I'd never have contemplated a shift, neither would I have imagined moving would embody the sweetness and joy that Ruby brought into my development life. It is literally structured in a way that makes developers feel happy, fewer lines of code, lots of gems and support. Its creator, [Yukihiro Matsumoto](https://en.wikipedia.org/wiki/Yukihiro_Matsumoto) must have created it to make developers happy.

![ruby](/assets/images/blog/php-ruby/ruby.jpg){:.img-responsive .center}

Prior to the sweetness came the adaptation, understanding how Ruby is structured, it's installation, gems (ruby libraries - literally has a gem for most things a developer could want) and the Ruby on Rails framework.

![phptoruby](/assets/images/blog/php-ruby/phptoruby.png){:.img-responsive .center}

I believe that it would be easier for the newbies in programming to dive into Ruby than for intermediate programmers switching to ruby. The challenge for the intermediate programmers is "force of habit". They are more likely to write the new language code with their old language style, principles and philosophies. In my case, i had to unlearn and unset my expectations brought about by coding in PHP. In the end the mindshift was worth it, allowing me to use my experience in coding to fly through Ruby.     

But hey, don't be scared, I just said ruby brought much joy to my development life and the two languages are kind of similar in some ways. 

### Here are the ways in which the 2 languages are similar:
- They are both interpreted languages
- They are both Object Oriented languages with classes, variables and methods having the 3 access control methods - private, protected and public; abstraction, inheritance and other OOP features working the same way
- They are both dynamically typed languages, so no declaring variable types like in java. You simply declare a variable For example:

```ruby
age = 10 #ruby
$age = 10 #PHP
```
- Both have `true` and `false`
- Arrays and hashes work in the same way

### Differences?
Definitely, yes! The easily notable differences between the 2 languages include:
- ruby files use `.rb` while PHP use `.php`
- commenting

```php
# this is a comment in ruby 
// this is a comment in php
```
- Everything in Ruby is an object including strings, numbers, hashes, arrays. That's why when converting between 2 data types, oftenly called as type casting, here's what happens:

```php
age.to_s # in ruby

strval($age) # in PHP or 
(string)$age 
```
```ruby
age.length # in ruby

strlen($age) # in PHP
```
- parentheses are not necessary in method calls unless arguments are required, unlike in PHP where you need to include parentheses in method calls. For example:

```ruby
person.get_age #method call in ruby

$person->age() #method call in PHP
```
- ruby doesn't have statement terminators like `semicolon(;)` in PHP. Starting a statement on a new line depicts end of statement
- `dot(.)` operator is used for concatenation in PHP unlike ruby which uses `plus(+)`. For example:

```ruby
# PHP
$age=10
echo "I am ".$age." years old" 

# ruby
age=10
puts "I am "+age.to_s+" years old" # note the explicit type conversion in ruby. Unlike in # PHP which does an implicit type conversion 
```
- ruby uses `do and end` pair keyword as well as `braces {}` to show code blocks, PHP uses `braces {}` only to show code blocks
- in ruby [duck typing](https://en.wikipedia.org/wiki/Duck_typing) is the way to go while in PHP either type hinting - ***defining type of arguments to pass in a method call*** or duck typing can be used but with care on the kinds of exceptions that may be raised.

```ruby
# duck typing in ruby
def get_age(person)
    return person.age
end

# type hinting in PHP
function get_age(PersonObj $person){
    return $person->age()
}
```
- debugging in PHP can get a developer in deep pain. If using Xdebug things can get really tough especially when setting it up. If not using Xdebug, var_dump() is an option but this entails numerous browser refreshing which is just a nuisance. Debugging in ruby is simplified. Ruby has got several gems specifically for code debugging. Such gems include byebug that are very simple to use. You simply bundle install the gem, include it in the development and test group and use it by calling `binding.pry` on a file you want to debug.
- shorter lines of code in basic programming statements like for loops. 

```ruby
# php
for ($num = 1; $num <= 10; $num ++) { 
    echo "$num \n"; 
}

# ruby
(0..10).each do |i| puts "#{i}" end
```
- exception handling in PHP uses the `try/catch` block while ruby uses `begin/rescue` blocks

```ruby
# in PHP
try{
    do_something();
}catch(Exception $e){
    echo "exception handled"+$e
}

# in ruby
begin
    do_something
rescue SomeException
    puts "exception handled"
end
```
- in PHP, composer is used to manage application libraries while ruby uses bundler that manages the ruby gems, tracking versions compatibility of the gems and updates the gems upon availability of newer versions.
- function declaration in PHP is done using `function` keyword enclosing the function body in braces while ruby uses `def` keyword as follow:

```ruby
# in PHP
function get_age(){
    echo $age
}

# in Ruby
def get_age
    puts age
end
```
- and so much other differences in things like arrays, hashes(called associative arrays in PHP), control statements, OOP features, namespacing

## Why Ruby

Let's now delve into the delicious parts of ruby.

### Code Quality
Reliability, maintainability and readability all characterize good code. Ruby was designed in a way that made achieving high quality code easier. A ruby developer doesn't even require years of experience before writing good code. With practices like separation of business logic from MVC implementation, use of problem detection gems and test automation, writing good code is assured.
### Tools
Ruby provides a [repository](https://rubygems.org/) of libraries called gems that technically help developers do more in less time. There's a gem for almost anything you could think of. Think of any functionality, type it into the Ruby repo and boom, you'll be surprised of the number of options available. Another plus is that gem installation is quite simple. All you need to do is type into your command line:
```ruby
gem install gem-name
# or simply include it into a Gemfile and use bundler by running
bundle install
```

### Community
There is an ever growing [Ruby community](https://www.ruby-lang.org/en/community/conferences/) with ruby boasting of large developer communities [existing in almost every part of the world](https://rubyconferences.org/). One close to my locality is [Nairuby](http://nairuby.org/). The repos on Github are also on the rise. The good thing with the Ruby community in Kenya is that those who choose it, understand it better than their counterparts in other languages because they are pioneers, so expect support from people having a deeper understanding of the language

### Test automation
Ruby is widely known for its TDD approach in development. It heavily focuses on testing and makes the process simple. It boasts of simple to use test automation tools like Minitest and RSpec that allow developers to write automated tests easily. Here's an example in Minitest:
```ruby
test "A is equal to B" do
    assert_equal(A, B)
end
```
### Security
Ruby also provides tools that help ensure the security of your application. With tools like Brakeman and Rubocop, static code analysis is enabled. This helps ensure that a developer writes code that is tamper-proof and prevent hacker activities in production.

### Built for the future

With many startups globally preferring Ruby as their primary programming languages, I would say Ruby was built for the future. This trend has been fueled by the MVC philosophy pioneered by Ruby on Rails and is now being adopted by other languages in the form of Laravel, Meteor etc. Statistics also show that most developers are interested in learning Ruby. Online learning platforms are also getting more people joining Ruby classes now than before. This just proves that in the coming years Ruby will take the lead. 

if you don't believe me, here is a google trends comparison between [Rails and Laravel in the US](https://trends.google.com/trends/explore?geo=US&q=%2Fm%2F0505cl,laravel){:data-proofer-ignore=''}, which is a trendsetting market in technology. 

![Laravel vs Rails](/assets/images/blog/php-ruby/laravelvrails-us.png){:.img-responsive .center}

The [Kenyan trend results](https://trends.google.com/trends/explore?geo=KE&q=%2Fm%2F060kv,%2Fm%2F06ff5){:data-proofer-ignore=''} are quite worrisome and it seems developers are still stuck in legacy and haven't been enlightened enough to make the move. Better get ready for the future!

![Laravel vs Rails](/assets/images/blog/php-ruby/laravelvrails-ke.png){:.img-responsive .center}

### Opportunity
Notable companies in the global and local markets using Ruby and RoR include:

#### Global Companies
[Github](https://github.com/)<br>
[Airbnb](http://airbnb.com/)<br>
[Fiverr](https://www.fiverr.com/)<br>
[Bloomberg](https://www.bloomberg.com/)<br>
[Crunchbase](https://www.crunchbase.com/){:data-proofer-ignore=''}

#### Local Companies
[Zegetech](https://zegetech.com/)<br>
[Andela](https://andela.com/)<br>
[Fuzu](https://www.fuzu.com/)<br>
[Kopokopo](https://kopokopo.co.ke/)<br>
[Finplus](https://finplusgroup.com/)<br>
[Ongair](https://ongair.im/)<br>
[Sokowatch](https://sokowatch.com/)<br>
[Tezza](https://www.tezzasolutions.com/)<br>
[Lipa Later](https://lipalater.com/)

Everyone is moving to Ruby, why don't you? 

## PHP vs Ruby

|| PHP | Ruby |
| --- |---|---|
| Github Repos| 138,000 | 132,000 |
| Famous sites| Facebook, Udemy, Wikipedia | Twitter, Hulu |
| Popularity| More popular(large developer community) | Less popular |
| Jobs Posting| 21% | 3% |
| Ease of development| Takes time | Saves time |
| Famous Web Framework| Laravel, others: codeigniter | Ruby on Rails, others: Sinatra |

## Takeaway

Don't be lured into the jackpot thing that brings instant gratification because it never works. Ever heard of **RoR in 7 days**? As [Peter Norvig](https://en.wikipedia.org/wiki/Peter_Norvig) puts it, those are SCAMs. When transitioning to language B, 
1. Start with a beginners mindset, have a book in place or official docs so as to understand the architecture of the syntax and semantics and read lots of code in that language from hubs like github, codeplex etc 
2. Understand the programming paradigm
3. Do small but useful projects in that particular language
4. Have learning timeboxes and deploy ready timeboxes
5. Don’t cram the syntax it will eventually stick. You can blog about something new you learnt, which will come in handy in future, so that if you get hazy (it happens), you can quickly refer to the blog piece (like zegetech blog), instead of jumping right to googling where you may get varied solutions of your problem
6. Recreate something from your “used-to” language in your new language, seek a mentor and just practice.  

Try to switch to Ruby today! There are  💎💎💎at the end of the 🌈 

![Try](/assets/images/blog/php-ruby/try.gif){:.img-responsive .center}

### *Extras* for you

[Why Ruby on Rails](2018-10-17-why-ruby-on-rails.md)<br>
[Deploy Rails](2018-11-20-deploy-rails.md)<br>
[Ruby Method Arguments](2019-02-28-ruby-method-arguments.md)<br>
[Getting started with Ruby on Rails](https://guides.rubyonrails.org/getting_started.html)

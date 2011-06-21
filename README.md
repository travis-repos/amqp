# About Ruby amqp gem #

Ruby amqp gem is a widely used, feature-rich, well-maintained asynchronous AMQP 0.9.1 client with batteries included.
This library works with Ruby 1.8.7 (p174 and p334), Ruby 1.9.2, [JRuby](http://jruby.org) (highly recommended to Microsoft Windows users), [REE](http://www.rubyenterpriseedition.com) and [Rubinius](http://rubini.us), and is licensed under the [Ruby License](http://www.ruby-lang.org/en/LICENSE.txt)

Versions 0.8.0 and later of amqp gem implement [AMQP 0.9.1](http://bit.ly/hw2ELX) and supports [RabbitMQ extensions to AMQP 0.9.1](http://www.rabbitmq.com/extensions.html).

[![Continuous Integration status](http://travis-ci.org/ruby-amqp/amqp.png)](http://travis-ci.org/ruby-amqp/amqp)


## I know what AMQP is, how do I get started? ##

See [Getting started with amqp gem](http://bit.ly/getting-started-with-amqp-ruby-gem) and other [documentation guides](http://bit.ly/amqp-gem-docs).



## What is AMQP? ##

AMQP is an [open standard for messaging middleware](http://www.amqp.org/confluence/display/AMQP/About+AMQP) that
emphasizes interoperability between different technologies (for example, Java, .NET, Ruby, Python, Node.js, Erlang, C and so on).

Key features of AMQP are very flexible yet simple routing and binary protocol efficiency. AMQP supports many sophisticated features, for example,
message acknowledgements, returning messages to producer, delivery confirmation, flow control and so on.


## What is amqp gem good for? ##

One can use amqp gem to make Ruby applications interoperate with other
applications (both Ruby and not). Complexity and size may vary from
simple work queues to complex multi-stage data processing workflows that involve
dozens or hundreds of applications built with all kinds of technologies.

Specific examples:

 * A Web application may route messages to a Java app that works
   with SMS delivery gateways.

 * Periodically run (Cron-driven) application may notify other systems that
   there are some new results.

 * Content aggregators may update full-text search and geospatial search indexes
   by delegating actual indexing work to other applications over AMQP.

 * Companies may provide streaming/push APIs to their customers, partners
   or just general public.

 * A new shiny Ruby-based system may be integrated with an existing C++-based component
   using AMQP.

 * An application that watches updates from a real-time stream (be it markets data
   or Twitter stream) can propagate updates to interested parties, including
   Web applications that display that information in the real time.



## Getting started with Ruby amqp gem ##

### Install RabbitMQ ###

Please refer to the [RabbitMQ installation guide](http://www.rabbitmq.com/install.html). Note that for Ubuntu and Debian we strongly advice that you
use [RabbitMQ apt repository](http://www.rabbitmq.com/debian.html#apt) that has recent versions of RabbitMQ. RabbitMQ packages Ubuntu and Debian ship
with are outdated even in recent (10.10) releases. Learn more in the [RabbitMQ versions guide](http://rubydoc.info/github/ruby-amqp/amqp/master/file/docs/RabbitMQVersions.textile).


### Install the gem ###

On Microsoft Windows 7

    gem install eventmachine --pre
    gem install amqp --pre --version "~> 0.8.0.RC12"

On other OSes or [JRuby](http://jruby.org):

    gem install amqp --pre --version "~> 0.8.0.RC12"


### "Hello, World" example ###

    #!/usr/bin/env ruby
    # encoding: utf-8

    require "rubygems"
    # or
    #
    # require "bundler"
    # Bundler.setup
    #
    # if you use Bundler

    require 'amqp'

    EventMachine.run do
      connection = AMQP.connect(:host => '127.0.0.1')
      puts "Connected to AMQP broker. Running #{AMQP::VERSION} version of the gem..."

      channel  = AMQP::Channel.new(connection)
      queue    = channel.queue("amqpgem.examples.hello_world", :auto_delete => true)
      exchange = channel.direct("")

      queue.subscribe do |payload|
        puts "Received a message: #{payload}. Disconnecting..."

        connection.close {
          EventMachine.stop { exit }
        }
      end

      exchange.publish "Hello, world!", :routing_key => queue.name
    end


[Getting started guide](http://bit.ly/getting-started-with-amqp-ruby-gem) explains this and two more examples in detail,
and is written in a form of a tutorial.



## Documentation: tutorials, guides & API reference ##

We believe that in order to be a library our users **really** love, we need to care about documentation as much as (or more)
code readability, API beauty and autotomated testing across 5 Ruby implementations on multiple operating systems. We do care
about our documentation: **if you don't find your answer in documentation, we consider it a high severity bug** that you
should [file to us](http://github.com/ruby-amqp/amqp/issues). Or just complain to [@rubyamqp](https://twitter.com/rubyamqp) on Twitter.


### Tutorials ###

[Getting started guide](http://bit.ly/getting-started-with-amqp-ruby-gem) is written as a tutorial that walks you through
3 examples:

 * The "Hello, world" of messaging, 1-to-1 communication
 * Blabbr, a Twitter-like example of broadcasting (1-to-many communication)
 * Weathr, an example of sophisticated routing capabilities AMQP 0.9.1 has to offer (1-to-many or many-to-many communication)

all in under 20 minutes. Check it out! If something isn't clear, every guide explains how to contact documentation authors at the bottom
of the page.


### Examples ###

You can find many examples (both real-world cases and simple demonstrations) under [examples directory](https://github.com/ruby-amqp/amqp/tree/master/examples) in the repository.
Note that those examples are written against version 0.8.0.rc1 and later. 0.6.x and 0.7.x may not support certain AMQP protocol or "DSL syntax" features.


### Documentation guides ###

[Documentation guides](http://bit.ly/amqp-gem-docs) describe the library itself as well as AMQP usage scenarios, topics like routing, error handing & recovery, broker-specific extensions, TLS support, troubleshooting and so on.


### API reference ###

[API reference](http://bit.ly/mDm1JE) is up on [rubydoc.info](http://rubydoc.info) and is updated daily.




## How to use AMQP gem with Ruby on Rails, Merb, Sinatra and other web frameworks ##

We cover this subject for multiple Ruby application servers in [Connecting to the broker guide](http://bit.ly/kFCVQU), take a look and let us know
what wasn't clear.



## Community

 * Join [Ruby AMQP mailing list](http://groups.google.com/group/ruby-amqp)
 * Follow [@rubyamqp](https://twitter.com/rubyamqp) on Twitter for Ruby AMQP ecosystem updates.
 * Join also [RabbitMQ mailing list](https://lists.rabbitmq.com/cgi-bin/mailman/listinfo/rabbitmq-discuss) (AMQP community epicenter).
 * Stop by #rabbitmq on irc.freenode.net. You can use [Web IRC client](http://webchat.freenode.net?channels=rabbitmq) if you don't have an IRC client installed.



## Links ##

* [API reference](http://rdoc.info/github/ruby-amqp/amqp/master/frames)
* [Documentation guides](http://bit.ly/amqp-gem-docs)
* [Code Examples](https://github.com/ruby-amqp/amq-protocol/tree/master/examples/)
* [Issue tracker](http://github.com/ruby-amqp/amqp/issues)
* [Continous integration status](http://travis-ci.org/#!/ruby-amqp/amqp)


## License ##

AMQP gem is licensed under the [Ruby License](http://www.ruby-lang.org/en/LICENSE.txt).



## Credits and copyright information ##

* The Original Code is [tmm1/amqp](http://github.com/tmm1/amqp).
* The Initial Developer of the Original Code is Aman Gupta.
* Copyright (c) 2008 - 2010 [Aman Gupta](http://github.com/tmm1).
* Contributions from [Jakub Stastny](http://github.com/botanicus) are Copyright (c) 2011 VMware, Inc.
* Copyright (c) 2010 — 2011 [ruby-amqp](https://github.com/ruby-amqp) group members.

Currently maintained by [ruby-amqp](https://github.com/ruby-amqp) group members
Special thanks to Dmitriy Samovskiy, Ben Hood and Tony Garnock-Jones.


## How can I learn more about AMQP and messaging in general? ##

### AMQP resources ###

 * [RabbitMQ tutorials](http://www.rabbitmq.com/getstarted.html) that demonstrate interoperability
 * [Wikipedia page on AMQP](http://en.wikipedia.org/wiki/Advanced_Message_Queuing_Protocol)
 * [AMQP quick reference](http://www.rabbitmq.com/amqp-0-9-1-quickref.html)
 * John O'Hara on the [history of AMQP](http://www.acmqueue.org/modules.php?name=Content&pa=showpage&pid=485)

### Messaging and distributed systems resources ###

 * [Enterprise Integration Patterns](http://www.eaipatterns.com), a book about messaging and use of messaging in systems integration.

 * [A Critique of the Remote Procedure Call Paradigm](http://www.cs.vu.nl/~ast/publications/euteco-1988.pdf)
 * [A Note on Distributed Computing](http://research.sun.com/techrep/1994/smli_tr-94-29.pdf)
 * [Convenience Over Correctness](http://steve.vinoski.net/pdf/IEEE-Convenience_Over_Correctness.pdf)
 * Joe Armstrong on [Erlang messaging vs RPC](http://armstrongonsoftware.blogspot.com/2008/05/road-we-didnt-go-down.html)



## (Very) Short FAQ ##

### So, does amqp gem only work with RabbitMQ? ###

This library is developed and tested primarily with [RabbitMQ](http://rabbitmq.com), although it should be compatible with any
server implementing the [AMQP 0.9.1 spec](http://bit.ly/hw2ELX). For AMQP 0.8 brokers, use amqp gem version 0.7.x.

### Why isn't Ruby 1.8.7.-p249 supported? Will it be supported in the future? ###

In order to make code like the following (pseudo-synchronous) work

    conn = AMQP.connect
    ch   = AMQP::Channel.new(conn)

    ex   = ch.default_exchange
    ex.publish(some_data)

and not be affected by this [Ruby 1.8.7-p249-specific bug (super called outside of method)](http://bit.ly/iONBmH), we need to
avoid any inheritance for key amqp gem classes: Channel, Queue, Exchange. This will take a moderate refactoring effort, and
is likely to happen in 0.8.0.RC15.


### How does amqp gem relate to amq-client gem, amq-protocol and libraries like bunny? ###

See [this page about AMQP gems family](https://github.com/ruby-amqp/amq-client/blob/master/README.textile)

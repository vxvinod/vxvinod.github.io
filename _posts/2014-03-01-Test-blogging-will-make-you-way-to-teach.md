---
layout: post
title: "Test bloogin will make you to become a tutor"
date: 2014-3-01
desc:"hellofdgdgdfg
ddgdfgdfgdfgdfg
dfgdfgdfg"
---

Ruby on Rails, often simply Rails, is an open source web application framework which runs via the Ruby programming language. It is a full-stack framework: it allows creating pages and applications that gather information from the web server, talk to or query the database, and render templates out of the box. As a result, Rails features a routing system that is independent of the web server.
Ruby on Rails emphasizes the use of well-known software engineering patterns and principles, such as active record pattern, convention over configuration (CoC), don't repeat yourself (DRY), and model–view–controller (MVC).
Contents  [hide] 
1 History
2 Technical overview
2.1 Framework structure
2.2 Deployment
3 Philosophy and design
4 Trademarks
5 Reception
6 See also
7 References
8 Bibliography
9 External links

History[edit]

David Heinemeier Hansson extracted Ruby on Rails from his work on Basecamp, a project management tool by 37signals (now a web application company).[3] Hansson first released Rails as open source in July 2004, but did not share commit rights to the project until February 2005.[4] In August 2006, the framework reached a milestone when Apple announced that it would ship Ruby on Rails with Mac OS X v10.5 "Leopard",[5] which was released in October 2007.
Rails version 2.3 was released on March 15, 2009 with major new developments in templates, engines, Rack and nested model forms. Templates enable the developer to generate a skeleton application with custom gems and configurations. Engines give developers the ability to reuse application pieces complete with routes, view paths and models. The Rack web server interface and Metal allow one to write optimized pieces of code that route around ActionController.[6]
On December 23, 2008, Merb, another web application framework, was launched, and Ruby on Rails announced it would work with the Merb project to bring "the best ideas of Merb" into Rails 3, ending the "unnecessary duplication" across both communities.[7] Merb was merged with Rails as part of the Rails 3.0 release.[8][9]
Rails 3.1 was released on August 31, 2011, featuring Reversible Database Migrations, Asset Pipeline, Streaming, jQuery as default JavaScript library and newly introduced CoffeeScript and Sass into the stack.[10]
Rails 3.2 was released on January 20, 2012 with a faster development mode and routing engine (also known as Journey engine), Automatic Query Explain and Tagged Logging.[11] Rails 3.2.x is the last version that supports Ruby 1.8.7.[12] Rails 3.2.12 supports Ruby 2.0[13]
Rails 4.0 was released on June 25, 2013, introducing Russian Doll Caching, Turbolinks, Live Streaming as well as making Active Resource, Active Record Observer and other components optional by splitting them as gems.[14]
Version history
Version	Date
1.0[15]	December 13, 2005
1.2[16]	January 19, 2007
2.0[17]	December 7, 2007
2.1[18]	June 1, 2008
2.2[19]	November 21, 2008
2.3[20]	March 16, 2009
3.0[21]	August 29, 2010
3.1[22]	August 31, 2011
3.2[23]	January 20, 2012
4.0[24]	June 25, 2013

Technical overview[edit]

Like many web frameworks, Ruby on Rails uses the model–view–controller (MVC) pattern to organize application programming.
In a default configuration, a model in the Ruby on Rails framework maps to a table in a database, and a Ruby file. For example, a model class User will usually be defined in the file user.rb in the app/models directory, and is linked to the table users in the database. Developers can choose any model name, file name or database table name. But this is not common practice and usually discouraged according to the "convention over configuration" philosophy.
A controller is a component of Rails that responds to external requests from the web server to the application by determining which view file to render. The controller may also have to query one or more models directly for information and pass these on to the view. A controller may provide one or more actions. In Ruby on Rails, an action is typically a basic unit that describes how to respond to a specific external web-browser request. Also note that the controller/action will be accessible for external web requests only if a corresponding route is mapped to it. Rails encourages developers to use RESTful routes, which include actions such as: create, new, edit, update, destroy, show, and index. These mappings of incoming requests/routes to controller actions can be easily set up in the routes configuration file.
A view in the default configuration of Rails is an erb file, which is converted to HTML at run-time. Many other templating systems can be used for views.
Ruby on Rails includes tools that make common development tasks easier "out of the box", such as scaffolding that can automatically construct some of the models and views needed for a basic website.[25] Also included are WEBrick, a simple Ruby web server that is distributed with Ruby, and Rake, a build system, distributed as a gem. Together with Ruby on Rails, these tools provide a basic development environment.
Ruby on Rails is most commonly not connected to the Internet directly, but through some front-end web server. Mongrel was generally preferred over WEBrick in the early days,[citation needed] but it can also run on Lighttpd, Apache, Cherokee, Hiawatha, nginx (either as a module — Phusion Passenger for example — or via CGI, FastCGI or mod_ruby), and many others. From 2008 onwards, the Passenger web server replaced Mongrel as the most-used web server for Ruby on Rails.[26]
Ruby on Rails is also noteworthy for its extensive use of the JavaScript libraries Prototype and Script.aculo.us for Ajax.[27] Ruby on Rails initially utilized lightweight SOAP for web services; this was later replaced by RESTful web services. Ruby on Rails 3.0 uses a technique called Unobtrusive JavaScript to separate the functionality (or logic) from the structure of the web page. jQuery is fully supported as a replacement for Prototype and is the default JavaScript library in Rails 3.1, reflecting an industry-wide move towards jQuery. Additionally, CoffeeScript was introduced in Rails 3.1 as the default Javascript language.
Since version 2.0, Ruby on Rails offers both HTML and XML as standard output formats. The latter is the facility for RESTful web services.
Rails 3.1 introduced Sass as standard CSS templating.
By default, the server uses Embedded Ruby in the HTML views, with files having an html.erb extension. Rails supports swapping-in alternative templating languages, such as HAML and Mustache.


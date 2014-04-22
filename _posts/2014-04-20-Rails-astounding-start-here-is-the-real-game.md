---
layout: post
title:  "Rails astounding start here is the real game"
date:   2014-04-20 
desc: "Sorry guys for not posting my learning as i was collecting the the things made the time wipe out of my eyes here we start the Rails with View part been discussed."

---


Sorry guys for not posting my learning as i was collecting the the things made the time wipe out of my eyes here we start the Rails with View part been discussed.

####View :

View is the simpelst part of MVC structure at the basic level it is just a HTML boilerplate where you can play with instance variable received from the controller and placing it to the browser.

we can explicitly tell the controller to call the particular view file using 'render' method.

####What is Layout ?

The common header and footer content in a webpage are been segregated in to seperated file called layout to avoid repeatablity of code.layout lies in 'app/views/layout'

for new Rails application 'application.html.erb' is the basic layout.usually navbars ,footers and snippets of code displaying flash is used in layout.

####Preprocessor:

Preprocessor is a program that process an data to produce an output and that output is used as input to another program.

ERB(Embedded Ruby) is a special way of executing ruby code inside HTML .

####Difference between <% %> and <%= %>:

<%= - it displays in webpage whatever returned in the erb tags. like <%= @user.firstname%> returns 'vinod' in the webpage.

<% - it is to execute purely ruby code like 'if' 'for' statements.

####Example

{%highlight ruby%}

    <% if current_user.signed_in? %>
      <ul>
        <% @users.each do |user| %>
          <li><%= user.first_name %></li>
        <% end %>
      </ul>
    <% else %>
      <strong>You must sign in!</strong>
    <% end %>

{%endhighlight%}

In the code above, if the user is signed in it will actually render to the web something like:

    <ul>
      <li>Bob</li>
      <li>Joe</li>
      <li>Nancy</li>
    </ul>

If the user isn't signed in, it'll be the much shorter:

    <strong>You must sign in!</strong>

####How Do Preprocessor work ?

ERB execution is done on the server and then final HTML file is been rendered to the browser. Rails knows it has to preprocess the file by seeing the extension '.html.erb'.

Rails start from outside in with extra extension here '.html.erb' erb is preprocessed then it is treated as HTML file.

'.css.scss' uses SASS preprocessor before coming to CSS file .

'js.coffee' uses Coffeescript preprocessor before regular javascript file.

preprocessor makes the life easier by allowing us to use loops working with variables.

####What is partials ?

common view template for several view files can be stored as layout.

it is stored with starting with '_' _user_form.html.erb

partials can ber rendered like
   # app/views/users/new.html.erb
    <div class="new-user-form">
      <%= render "user_form" %>
    <div>

 if partials are stored inside 'shared 'directory then it can be rendered using

 <%= render "shared/user_form" %>

 ####Using Local Variables in Hash:

 We can use @user local variable in partials but if more controllers are usign this partials surely it will end up in mess so in order to use local variables Rails lets you to pass options hash.

 :locals key in options hash will contain the variables to be passed.

 <%= render "shared/somepartials", :locals => {:user => @user} %>

 To use the variable in your partial file, you drop the @ and call it like a normal variable.

####Implicit Partials:

if we want to render all users list in User model then we can have a seperate partials which will do this .

index file will be like :
   # app/views/index.html.erb
    <h1>Users</h1>
    <ul>
      <% @users.each do |user| %>
        <%= render "user", :locals => {:user => user} %>
      <% end %>
    </ul>

partial _user.html.erb

  ####app/views/_user.html.erb
    <li><%= "#{user.first_name} #{user.last_name}, #{user.email}" %></li>


Rails Magic Way of rendering:
    <%= render "user", :locals => {:user => user} %>
instead of rendering and passing :locals explicitly. we can just say render user


   #### app/views/index.html.erb
    <h1>Users</h1>
    <ul>
      <% @users.each do |user| %>
        <%= render user %>     
      <% end %>
    </ul>

*Rails will render _user.html.erb and passes the 'user' variable automatically.

if we want to render whole bunch of users then 

  #### app/views/index.html.erb
    <h1>Users</h1>
    <ul>
      <%= render @users %>
    </ul>

this will automatically render _user.html.erb and iterated the users variable and prints it.

####Helper Methods:

Rails provide many handy helper methods can be accessed from view.

  <%= link_to "See All Users", users_path %>

  users_path = renders the '/users' URL
  users_url = renders the full URL.

####Asset Tags:

Rails provide helper methods to grab CSS and javascript files.

<%= stylesheet_link_tag "your_stylesheet" %>
    <%= javascript_include_tag "your_javascript" %>
    <%= image_tag "happy_cat.jpg" %>


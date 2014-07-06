---
layout: post
title:  "HTML CSSS and Javascript Notes"
date:   2014-07-05 
desc: "I felt that i need to refresh HTML CSS and Javascript basics so that it will help me while designing basic templates and HTML 5 impressed me alot."
---

###Jquery notes:

* call() and apply() - pass the object as parameter

say = person.firstName;

say.call(person); -- this will pass the person object and can be referenced by 'this' keyword.

* data

Any data that was passed in when the event was bound. For example:	
{% highlight ruby %}
// Event setup using the `.on()` method with data
$( "input" ).on(
"change",
{ foo: "bar" }, // associate data with event binding
function( eventObject ) {
console.log("An input value has changed! ", eventObject.data.foo);
}
);
{% endhighlight %}

* Multiple event sharing same functionalities
{% highlight ruby %}
// Multiple events, same handler
$( "input" ).on(
"click change", // bind listeners for multiple events
function() {
console.log( "An input was clicked or changed!" )
}
);

{%endhighlight%}

Posted in: Events
##jQuery Event Basics

link jQuery Event Basics
link Setting Up Event Responses on DOM Elements

jQuery makes it straightforward to set up event-driven responses on page elements. These events are often triggered by the end user's interaction with the page, such as when text is entered into a form element or the mouse pointer is moved. In some cases, such as the page load and unload events, the browser itself will trigger the event.

jQuery offers convenience methods for most native browser events. These methods — including .click(), .focus(), .blur(), .change(), etc. — are shorthand for jQuery's .on() method. The on method is useful for binding the same handler function to multiple events, when you want to provide data to the event handler, when you are working with custom events, or when you want to pass an object of multiple events and handlers.


// Event setup using a convenience method
$( "p" ).click(function() {
console.log( "You clicked a paragraph!" );
});



// Equivalent event setup using the `.on()` method
$( "p" ).on( "click", function() {
console.log( "click" );
});

####link Extending Events to New Page Elements

It is important to note that .on() can only create event listeners on elements that exist at the time you set up the listeners. Similar elements created after the event listeners are established will not automatically pick up event behaviors you've set up previously. For example:

	

$( document ).ready(function(){
// Sets up click behavior on all button elements with the alert class
// that exist in the DOM when the instruction was executed
$( "button.alert" ).on( "click", function() {
console.log( "A button with the alert class was clicked!" );
});
// Now create a new button element with the alert class. This button
// was created after the click listeners were applied above, so it
// will not have the same click behavior as its peers
$( "button" ).addClass( "alert" ).appendTo( document.body );
});

Consult the article on event delegation to see how to use .on() so that event behaviors will be extended to new elements without having to rebind them.
link Inside the Event Handler Function

Every event handling function receives an event object, which contains many properties and methods. The event object is most commonly used to prevent the default action of the event via the .preventDefault() method. However, the event object contains a number of other useful properties and methods, including:
link pageX, pageY

The mouse position at the time the event occurred, relative to the top left corner of the page display area (not the entire browser window).
link type

The type of the event (e.g. "click").
link which

The button or key that was pressed.
link data

Any data that was passed in when the event was bound. For example:


// Event setup using the `.on()` method with data
$( "input" ).on(
"change",
{ foo: "bar" }, // associate data with event binding
function( eventObject ) {
console.log("An input value has changed! ", eventObject.data.foo);
}
);

link target

The DOM element that initiated the event.
link namespace

The namespace specified when the event was triggered.
link timeStamp

The difference in milliseconds between the time the event occurred in the browser and January 1, 1970.
link preventDefault()

Prevent the default action of the event (e.g. following a link).
link stopPropagation()

Stop the event from bubbling up to other elements.

In addition to the event object, the event handling function also has access to the DOM element that the handler was bound to via the keyword this. To turn the DOM element into a jQuery object that we can use jQuery methods on, we simply do $( this ), often following this idiom:
1
	

var element = $( this );

A fuller example would be:


// Preventing a link from being followed
$( "a" ).click(function( eventObject ) {
var elem = $( this );
if ( elem.attr( "href" ).match( /evil/ ) ) {
eventObject.preventDefault();
elem.addClass( "evil" );
}
});

link Setting Up Multiple Event Responses

Quite often elements in your application will be bound to multiple events. If multiple events are to share the same handling function, you can provide the event types as a space-separated list to .on():

// Multiple events, same handler
$( "input" ).on(
"click change", // bind listeners for multiple events
function() {
console.log( "An input was clicked or changed!" )
}
);

* When each event has its own handler, you can pass an object into .on() with one or more key/value pairs, with the key being the event name and the value being the function to handle the event.

// Binding multiple events with different handlers
$( "p" ).on({
"click": function() { console.log( "clicked!" ); },
"mouseover": function() { console.log( "hovered!" ); }
});

* Tear down the listeners
// Tearing down all click handlers on a selection
$( "p" ).off( "click" );

*events for one click
// Switching handlers using the `$.fn.one` method
$( "p" ).one( "click", firstClick );
function firstClick() {
console.log( "You just clicked this for the first time!" );
// Now set up the new handler for subsequent clicks;
// omit this step if no further click responses are needed
$( this ).click( function() { console.log( "You have clicked this before!" ); } );
}

.one() can also be used to bind multiple events:	

// Using .one() to bind several events
$( "input[id]" ).one( "focus mouseover keydown", firstEvent);
function firstEvent( eventObject ) {
console.log( "A " + eventObject.type + " event occurred for the first time on the input with id " + this.id );
}

The .toggle() method is triggered by the "click" event and accepts two or more functions. Each time the click event occurs, the next function in the list is called. Generally, .toggle() is used with just two functions; however, it will accept an unlimited number of functions. Be careful, though: providing a long list of functions can be difficult to debug.

// The toggle helper function
$( "p.expander" ).toggle( function() {
$( this ).prev().addClass( "open" );
}, function() {
$( this ).prev().removeClass( "open" );
});

* 
Posted in: Events
Introducing Events
link Introduction

Web pages are all about interaction. Users perform a countless number of actions such as moving their mice over the page, clicking on elements, and typing in textboxes — all of these are examples of events. In addition to these user events, there are a slew of others that occur, like when the page is loaded, when video begins playing or is paused, etc. Whenever something interesting occurs on the page, an event is fired, meaning that the browser basically announces that something has happened. It's this announcement that allows developers to "listen" for events and react to them appropriately.
link What's a DOM event?

As mentioned, there are a myriad of event types, but perhaps the ones that are easiest to understand are user events, like when someone clicks on an element or types into a form. These types of events occur on an element, meaning that when a user clicks on a button for example, the button has had an event occur on it. While user interactions aren't the only types of DOM events, they're certainly the easiest to understand when starting out. MDN has a good reference of available DOM events.
link Ways to listen for events

There are many ways to listen for events. Actions are constantly occurring on a webpage, but the developer is only notified about them if they're listening for them. Listening for an event basically means you're waiting for the browser to tell you that a specific event has occurred and then you'll specify how the page should react.

To specify to the browser what to do when an event occurs, you provide a function, also known as an event handler. This function is executed whenever the event occurs (or until the event is unbound).

For instance, to alert a message whenever a user clicks on a button, you might write something like this:
1
	

<button onclick="alert('Hello')">Say hello</button>

The event we want to listen to is specified by the button's onclick attribute, and the event handler is the alert function which alerts "Hello" to the user. While this works, it's an abysmal way to achieve this functionality for a couple of reasons:

    First, we're coupling our view code (HTML) with our interaction code (JS). That means that whenever we need to update functionality, we'd have to edit our HTML which is just a bad practice and a maintenance nightmare.
    Second, it's not scalable. If you had to attach this functionality onto numerous buttons, you'd not only bloat the page with a bunch of repetitious code, but you would again destroy maintainability.

Utilizing inline event handlers like this can be considered obtrusive JavaScript, but its opposite, unobtrusive JavaScript is a much more common way of discussing the topic. The notion of unobtrusive JavaScript is that your HTML and JS are kept separate and are therefore more maintainable. Separation of concerns is important because it keeps like pieces of code together (i.e. HTML, JS, CSS) and unlike pieces of code apart, facilitating changes, enhancements, etc. Furthermore, unobtrusive JavaScript stresses the importance of adding the least amount of cruft to a page as possible. If a user's browser doesn't support JavaScript, then it shouldn't be intertwined into the markup of the page. Also, to prevent naming collisions, JS code should utilize a single namespace for different pieces of functionality or libraries. jQuery is a good example of this, in that the jQuery object/constructor (and also the $ alias to jQuery) only utilizes a single global variable, and all of jQuery's functionality is packaged into that one object.

To accomplish the desired task unobtrusively, let's change our HTML a little bit by removing the onclick attribute and replacing it with an id, which we'll utilize to "hook onto" the button from within a script file.
1
	

<button id="helloBtn">Say hello</button>

If we wanted to be informed when a user clicks on that button unobtrusively, we might do something like the following in a separate script file:


// Event binding using addEventListener
var helloBtn = document.getElementById( "helloBtn" );
helloBtn.addEventListener( "click", function( event ) {
alert( "Hello." );
}, false );

Here we're saving a reference to the button element by calling getElementById and assigning its return value to a variable. We then call addEventListener and provide an event handler function that will be called whenever that event occurs. While there's nothing wrong with this code as it will work fine in modern browsers, it won't fare well in versions of IE prior to IE9. This is because Microsoft chose to implement a different method, attachEvent, as opposed to the W3C standard addEventListener, and didn't get around to changing it until IE9 was released. For this reason, it's beneficial to utilize jQuery because it abstracts away browser inconsistencies, allowing developers to use a single API for these types of tasks, as seen below.


// Event binding using a convenience method
$( "#helloBtn" ).click(function( event ) {
alert( "Hello." );
});

The $( "#helloBtn" ) code selects the button element using the $ (a.k.a. jQuery) function and returns a jQuery object. The jQuery object has a bunch of methods (functions) available to it, one of them named click, which resides in the jQuery object's prototype. We call the click method on the jQuery object and pass along an anonymous function event handler that's going to be executed when a user clicks the button, alerting "Hello." to the user.

There are a number of ways that events can be listened for using jQuery:

// The many ways to bind events with jQuery
// Attach an event handler directly to the button using jQuery's
// shorthand `click` method.
$( "#helloBtn" ).click(function( event ) {
alert( "Hello." );
});
// Attach an event handler directly to the button using jQuery's
// `bind` method, passing it an event string of `click`
$( "#helloBtn" ).bind( "click", function( event ) {
alert( "Hello." );
});
// As of jQuery 1.7, attach an event handler directly to the button
// using jQuery's `on` method.
$( "#helloBtn" ).on( "click", function( event ) {
alert( "Hello." );
});
// As of jQuery 1.7, attach an event handler to the `body` element that
// is listening for clicks, and will respond whenever *any* button is
// clicked on the page.
$( "body" ).on({
click: function( event ) {
alert( "Hello." );
}
}, "button" );
// An alternative to the previous example, using slightly different syntax.
$( "body" ).on( "click", "button", function( event ) {
alert( "Hello." );
});

As of jQuery 1.7, all events are bound via the on method, whether you call it directly or whether you use an alias/shortcut method such as bind or click, which are mapped to the on method internally. With this in mind, it's beneficial to use the on method because the others are all just syntactic sugar, and utilizing the on method is going to result in faster and more consistent code.

Let's look at the on examples from above and discuss their differences. In the first example, a string of click is passed as the first argument to the on method, and an anonymous function is passed as the second. This looks a lot like the bind method before it. Here, we're attaching an event handler directly to #helloBtn. If there were any other buttons on the page, they wouldn't alert "Hello" when clicked because the event is only attached to #helloBtn.

In the second on example, we're passing an object (denoted by the curly braces {}), which has a property of click whose value is an anonymous function. The second argument to the on method is a jQuery selector string of button. While examples 1–3 are functionally equivalent, example 4 is different in that the body element is listening for click events that occur on any button element, not just #helloBtn. The final example above is exactly the same as the one preceding it, but instead of passing an object, we pass an event string, a selector string, and the callback. Both of these are examples of event delegation, a process by which an element higher in the DOM tree listens for events occurring on its children.

Event delegation works because of the notion of event bubbling. For most events, whenever something occurs on a page (like an element is clicked), the event travels from the element it occurred on, up to its parent, then up to the parent's parent, and so on, until it reaches the root element, a.k.a. the window. So in our table example, whenever a td is clicked, its parent tr would also be notified of the click, the parent table would be notified, the body would be notified, and ultimately the window would be notified as well. While event bubbling and delegation work well, the delegating element (in our example, the table) should always be as close to the delegatees as possible so the event doesn't have to travel way up the DOM tree before its handler function is called.

The two main pros of event delegation over binding directly to an element (or set of elements) are performance and the aforementioned event bubbling. Imagine having a large table of 1,000 cells and binding to an event for each cell. That's 1,000 separate event handlers that the browser has to attach, even if they're all mapped to the same function. Instead of binding to each individual cell though, we could instead use delegation to listen for events that occur on the parent table and react accordingly. One event would be bound instead of 1,000, resulting in way better performance and memory management.

The event bubbling that occurs affords us the ability to add cells via AJAX for example, without having to bind events directly to those cells since the parent table is listening for clicks and is therefore notified of clicks on its children. If we weren't using delegation, we'd have to constantly bind events for every cell that's added which is not only a performance issue, but could also become a maintenance nightmare.
link The event object

In all of the previous examples, we've been using anonymous functions and specifying an event argument within that function. Let's change it up a little bit.


// Binding a named function
function sayHello( event ) {
alert( "Hello." );
}
$( "#helloBtn" ).on( "click", sayHello );

In this slightly different example, we're defining a function called sayHello and then passing that function into the on method instead of an anonymous function. So many online examples show anonymous functions used as event handlers, but it's important to realize that you can also pass defined functions as event handlers as well. This is important if different elements or different events should perform the same functionality. This helps to keep your code DRY.

But what about that event argument in the sayHello function — what is it and why does it matter? In all DOM event callbacks, jQuery passes an event object argument which contains information about the event, such as precisely when and where it occurred, what type of event it was, which element the event occurred on, and a plethora of other information. Of course you don't have to call it event; you could call it e or whatever you want to, but event is a pretty common convention.

If the element has default functionality for a specific event (like a link opens a new page, a button in a form submits the form, etc.), that default functionality can be cancelled. This is often useful for AJAX requests. When a user clicks on a button to submit a form via AJAX, we'd want to cancel the button/form's default action (to submit it to the form's action attribute), and we would instead do an AJAX request to accomplish the same task for a more seamless experience. To do this, we would utilize the event object and call its .preventDefault() method. We can also prevent the event from bubbling up the DOM tree using .stopPropagation() so that parent elements aren't notified of its occurrence (in the case that event delegation is being used).
	

// Preventing a default action from occurring and stopping the event bubbling
$( "form" ).on( "submit", function( event ) {
// Prevent the form's default submission.
event.preventDefault();
// Prevent event from bubbling up DOM tree, prohibiting delegation
event.stopPropagation();
// Make an AJAX request to submit the form data
});

When utilizing both .preventDefault() and .stopPropagation() simultaneously, you can instead return false to achieve both in a more concise manner, but it's advisable to only return false when both are actually necessary and not just for the sake of terseness. A final note on .stopPropagation() is that when using it in delegated events, the soonest that event bubbling can be stopped is when the event reaches the element that is delegating it.

* trigger the action 
// This will not change the current page

* .trigger() vs .triggerHandler()

There are four differences between .trigger() and .triggerHandler()

    .triggerHandler() only triggers the event on the first element of a jQuery object.
    .triggerHandler() cannot be chained. It returns the value that is returned by the last handler, not a jQuery object.
    .triggerHandler() will not cause the default behavior of the event (such as a form submission).
    Events triggered by .triggerHandler(), will not bubble up the DOM hierarchy. Only the handlers on the single element will fire.

$( "a" ).trigger( "click" );


<div class="room" id="kitchen">
<div class="lightbulb on"></div>
<div class="switch"></div>
<div class="switch"></div>
<div class="clapper"></div>
</div>

Triggering the clapper or either of the switches will change the state of the lightbulb. The switches and the clapper don't care what state the lightbulb is in; they just want to change the state.

Without custom events, you might write some code like this:

$( ".switch, .clapper" ).click(function() {
var light = $( this ).parent().find( ".lightbulb" );
if ( light.hasClass( "on" ) ) {
light.removeClass( "on" ).addClass( "off" );
} else {
light.removeClass( "off" ).addClass( "on" );
}
});

* With custom events, your code might look more like this:	

$( ".lightbulb" ).on( "changeState", function( e ) {
var light = $( this );
if ( light.hasClass( "on" ) ) {
light.removeClass( "on" ).addClass( "off" );
} else {
light.removeClass( "off" ).addClass( "on" );
}
});
$( ".switch, .clapper" ).click(function() {
$( this ).parent().find( ".lightbulb" ).trigger( "changeState" );
});

* <div class="room" id="kitchen">
<div class="lightbulb on"></div>
<div class="switch"></div>
<div class="switch"></div>
<div class="clapper"></div>
</div>
<div class="room" id="bedroom">
<div class="lightbulb on"></div>
<div class="switch"></div>
<div class="switch"></div>
<div class="clapper"></div>
</div>
<div id="master_switch"></div>

If there are any lights on in the house, we want the master switch to turn all the lights off; otherwise, we want it to turn all lights on. To accomplish this, we'll add two more custom events to the lightbulbs: turnOn and turnOff. We'll make use of them in the changeState custom event, and use some logic to decide which one the master switch should trigger:

$( ".lightbulb" ).on( "changeState", function( e ) {
var light = $( this );
if ( light.hasClass( "on" ) ) {
light.trigger( "turnOff" );
} else {
light.trigger( "turnOn" );
}
}).on( "turnOn", function( e ) {
$( this ).removeClass( "off" ).addClass( "on" );
}).on( "turnOff", function( e ) {
$( this ).removeClass( "on" ).addClass( "off" );
});
$( ".switch, .clapper" ).click(function() {
$( this ).parent().find( ".lightbulb" ).trigger( "changeState" );
});
$( "#master_switch" ).click(function() {
if ( $( ".lightbulb.on" ).length ) {
$( ".lightbulb" ).trigger( "turnOff" );
} else {
$( ".lightbulb" ).trigger( "turnOn" );
}
});


### Ajax:
The .serialize() method serializes a form's data into a query string. For the element's value to be serialized, it must have a name attribute. Please note that values from inputs with a type of checkbox or radio are included only if they are checked.

// Turning form data into a query string
$( "#myForm" ).serialize();
// creates a query string like this:
// field_1=something&field2=somethingElse

While plain old serialization is great, sometimes your application would work better if you sent over an array of objects, instead of just the query string. For that, jQuery has the .serializeArray() method. It's very similar to the .serialize() method listed above, except it produces an array of objects, instead of a string.

// Creating an array of objects containing form data
$( "#myForm" ).serializeArray();
// creates a structure like this:
// [
// {
// name : "field_1",
// value : "something"
// },
// {
// name : "field_2",
// value : "somethingElse"
// }
// ]

* With that being said, let's jump on in to some examples! First, we'll see how easy it is to check if a required field doesn't have anything in it. If it doesn't, then we'll return false, and prevent the form from processing.
	

// Using validation to check for the presence of an input
$( "#form" ).submit(function( event ) {
// if .required's value's length is zero
if ( $( ".required" ).val().length === 0 ) {
// usually show some kind of error message here
// this prevents the form from submitting
return false;
} else {
// run $.ajax here
}
});

Let's see how easy it is to check for invalid characters in a phone number:	

// Validate a phone number field
$( "#form" ).submit(function( event ) {
var inputtedPhoneNumber = $( "#phone" ).val();
// match only numbers
var phoneNumberRegex = /^\d*$/;
// if the phone number doesn't match the regex
if ( !phoneNumberRegex.test( inputtedPhoneNumber ) ) {
// usually show some kind of error message here
// prevent the form from submitting
return false;
} else {
// run $.ajax here
}
});

* // Using YQL and JSONP
$.ajax({
url: "http://query.yahooapis.com/v1/public/yql",
// the name of the callback parameter, as specified by the YQL service
jsonp: "callback",
// tell jQuery we're expecting JSONP
dataType: "jsonp",
// tell YQL what we want and that we want JSON
data: {
q: "select title,abstract,url from search.news where query=\"cat\"",
format: "json"
},
// work with the response
success: function( response ) {
console.log( response ); // server response
}
});


* JAVASCRIPT How to write 

https://github.com/airbnb/javascript#comments

* for pressing keys to get in javascript

function keysPressed(e) {
    // store an entry for every key pressed
    keys[e.keyCode] = true;
     
    // Ctrl + Shift + 5
    if (keys[17] && keys[16] && keys[53]) {
        // do something
    }
     
    // Ctrl + f
    if (keys[17] && keys[70]) {
        // do something
     
        // prevent default browser behavior
        e.preventDefault();
    }

* jQuery is all about selectors, which is why the load() function allows you to cut down on the returned HTML by defining a selector. This means that you can change the script to the following (you can try the selector filtering example for yourself):

$('.ajaxtrigger').click(function(){
  var url = $(this).attr('href');
  $('#target').load(url+' #bd blockquote');
  return false;
});

* write good ajax code 

$(document).ready(function(){
  var container = $('#target');
  $('.ajaxtrigger').click(function(){
    doAjax($(this).attr('href'));
    return false;
  });
  function doAjax(url){
    $.ajax({
      url: url,
      success: function(data){
        container.html(data);
      },
      beforeSend: function(data){
        container.html('<p>Loading...</p>');
      }
    });
  }
});


* Error Handling

As you may have guessed, the next logical step is to handle error cases. This is something far too many AJAX solutions haven’t gotten right, and seeing a great application become useless just because one call has timed out is very frustrating.

We have to prepare for three different errors:

    The user tries to load an external file that is not available because of AJAX security settings;
    There is some server error (for example, “Page not found”);
    The resource takes too long to load.

The following script takes care of all this, and you can see it in action on the error handling demo page.

$(document).ready(function(){
  var container = $('#target');
  $('.ajaxtrigger').click(function(){
    doAjax($(this).attr('href'));
    return false;
  });
  function doAjax(url){
    if(url.match('^http')){
      var errormsg = 'AJAX cannot load external content';
      container.html(errormsg);
    } else {
      $.ajax({
        url: url,
        timeout:5000,
        success: function(data){
          container.html(data);
        },
        error: function(req,error){
          if(error === 'error'){error = req.statusText;}
          var errormsg = 'There was a communication error: '+error;
          container.html(errormsg);
        },
        beforeSend: function(data){
          container.html('<p>Loading...</p>');
        }
      });
    }
  }
});

The changes are:

    We test whether the link URI starts with http and then report an error that loading it with AJAX is not possible.
    If the link doesn’t begin with http, we start a new AJAX request. This one has a few new features:
        We define a timeout of 5 seconds (i.e. 5000 milliseconds);
        We add an error handler.
    The error handler either sends us what happened on the server as req.statustext or gives us the error message timeout when the 5 seconds are up. So, we need to check what we got before we write out the error message.

Deciding Between <article>, <section>, or <div> Elements

At times it becomes fairly difficult to decide which element—<article>, <section>, or <div>—is the best element for the job based on its semantic meaning. The trick here, as with every semantic decision, is to look at the content.

Both the <article> and <section> elements contribute to a document’s structure and help to outline a document. If the content is being grouped solely for styling purposes and doesn’t provide value to the outline of a document, use the <div> element.

If the content adds to the document outline and it can be independently redistributed or syndicated, use the <article> element.

If the content adds to the document outline and represents a thematic group of content, use the <section> element.


* write best html css code 
http://learn.shayhowe.com/html-css/writing-your-best-code/

* add audio controls to play pause

<audio src="jazz.ogg" controls></audio>

<audio controls>
  <source src="jazz.ogg" type="audio/ogg">
  <source src="jazz.mp3" type="audio/mpeg">
  <source src="jazz.wav" type="audio/wav">
  Please <a href="jazz.mp3" download>download</a> the audio file.
</audio>

* add video with controls 

<video src="earth.ogv" controls></video>

* working with tables

http://learn.shayhowe.com/html-css/organizing-data-with-tables/

* forms
http://learn.shayhowe.com/html-css/building-forms/

* box model

http://learn.shayhowe.com/html-css/opening-the-box-model/

* design page

http://learnlayout.com/position-example.html

* css newspaper format 
http://learnlayout.com/column.html

*frameworks'
'http://learnlayout.com/frameworks.html

*background and gradients
http://learn.shayhowe.com/html-css/setting-backgrounds-and-gradients/

* font styling
http://learn.shayhowe.com/html-css/working-with-typography/

*Css Transition
http://alistapart.com/article/understanding-css3-transitions

*animation cheat sheat
http://www.justinaguilar.com/animations/#


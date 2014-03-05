---
layout: post
title:  "Jquery ui Dialog box displays the content by ajax its dead hot ghost rider"
date:   2014-03-05 
desc: "Jquery UI its mind blowing and stunning less code do more omg but i didnt expect it will do this much come lets explore the beauty of jquery wit ajax"
---

At many times i was eager to learn ajax,yeah obviously the beauty of it is 
it brings the data in a fraction of seconds without loading the page.at that time
i was searching for some good blogs over there and i came across a good blog where the 
author has explained how to pop up modal overlay dialog box and retrieve the contents 
asynchronously.it was mind blowing i cant believe it because this much less code does big things
that is jquery.

+ An options object can be used in a dialog's widget method to configure various dialog options

####Options	Default value	Usage
+ **autoOpen**	 true	Shows the dialog as soon as the dialog() method is called when set to true.

+ **bgiframe**	false -- Creates an <iframe> shim to prevent <select> elements showing through the dialog in IE6 (at present the bgiframe plugin is required). We'll look at this option in more detail shortly. This plugin is due to be retired in version 1.8 of the library and will be replaced by the new stackfix component.
+ **buttons**	{}	Supplies an object containing buttons to be used with the dialog. Each     
                                     property name becomes the text on the <button> element, and the value of each property is a callback function, which is executed when the button is clicked.
+ **closeOnEscape**	true -	If set to true, the dialog will close when the Esc key is pressed.
+ **dialogClass**	""	- Sets additional class names on the dialog for theming purposes.
+ **draggable**	true -	Makes the dialog draggable (use ui.draggable.js).
+ **height**	"auto" -	Sets the starting height of the dialog.
+ **hide**	null	-Sets an effect to be used when the dialog is closed.
+ **maxHeight**	false-	Sets a maximum height for the dialog.
+ **maxWidth**	false	-Sets a maximum width for the dialog.
+ **minHeight**	150 -	Sets a minimum height for the dialog.
+ **minWidth**	150 -	Sets a minimum width for the dialog.
+ **modal**	false	Enables modality while the dialog is open.
+ **position**	"center"	-Sets the starting position of the dialog in the viewport. Can accept a + string, or an array of strings, or an array containing exact coordinates of the dialog offset from the top and left of the viewport.
+ **resizable** true -	Makes the dialog resizable (also requires ui.resizable.js).
+ **show**	null	Sets an effect to be used when the dialog is opened.
+ **stack**	true-	Causes the focused dialog to move to the front when several dialogs are open.
+ **title**	false -	Alternative to specifying the title attribute on the widget's underlying container element.
+ **width**	300-	Sets the starting width of the dialog.
+ **zIndex**	1000--Sets the starting CSS z-index of the widget. When using multiple dialogs and the stack option is set to true the z-index will change as each dialog is moved to the front of the stack.

and i am damn sure you wont be able to get these compicated things so lets begin to practical work before that i will explain small snippet of code to make it clear

{%highlight javascript%}
	$("#div1").dialog({
					autoOpen:false,
					modal:true,   //open as modal overlay
					width: 600, //width size
					height:300, //height size
					buttons:{    //buttons in dialog box can be added
						"Dismiss": function(){     //adding dismiss button
							$(this).dialog("close");
						}
					}
					
})
{%endhighlight%}

Explanation:
 we are defining the modal dialog box to the div element and initializing with values such as it should be modal overlay,width and hieght of it and inserting some buttons and necessary actions in anonymous functions in dialog box .

{%highlight javascript%}
$("button#hitme").on('click',function(e){
				console.log("clicked");
				e.preventDefault();
				$("#div1").html("");
				$("#div1").dialog("option","title","Loading...").dialog("open");
				$("#div1").load("what_i_am.txt",function(){
					$("#div1").dialog("option","title",$(this).find("h2").text());
					$(this).find("h2").remove();
				});

{%%endhighlight}

Explanation:
+ $("#div1").dialog("option","title","Loading...").dialog("open"); --open the dialog box and set the title of it as Loading...
+ $("#div1").load("what_i_am.txt",function -- load the content of text file and insert it in to doalog box.
+ $("#div1").dialog("option","title",$(this).find("h2").text()); -- find the h2 tag in the text file and put it as title of dialogg box.
+ $(this).find("h2").remove(); -- remove the title from the body of dialog box.

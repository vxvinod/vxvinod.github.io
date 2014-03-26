---
layout: post
title:  "draggable UI made me craze"
date:   2014-03-26
desc: "Today was a fantastic day, was thinking to leave for the day but the promise i made was not fulfilled. so i stayed and completed the work and i feel happy to blog that. today we will play with play boeard where we are going to use our creativity and build new things using the available shapes.
"
---



Today was a fantastic day, was thinking to leave for the day but the promise i made was not fulfilled. so i stayed and completed the work and i feel happy to blog that. today we will play with play boeard where we are going to use our creativity and build new things using the available shapes.

####Explanation:
+ here we are going to hack the playground by creating 8 blcoks in each shape.
+ where we are creating 8 circle by using reapeted div.
+ we use draggable() in jquery Ui which helps to drag the shapes throughout body.
      + containment property in draggable specifies 'Constrains dragging to within the bounds of the specified element or region.
Multiple types supported:

    Selector: The draggable element will be contained to the bounding box of the first element found by the selector. If no element is found, no containment will be set.
    Element: The draggable element will be contained to the bounding box of this element.
    String: Possible values: "parent", "document", "window".
    Array: An array defining a bounding box in the form [ x1, y1, x2, y2 ].
'
	  + 'stack'- Controls the z-index of the set of elements that match the selector, always brings the currently dragged item to the front. Very useful in things like window managers.
	  + 'snap' - Whether the element should snap to other elements.
Multiple types supported:

    Boolean: When set to true, the element will snap to all other draggable elements.
    Selector: A selector specifying which elements to snap to.

	  + 'snap Tolerance'-The distance in pixels from the snap element edges at which snapping should occur. Ignored if the snap option is false.

	  + 'snap Mode' - Determines which edges of snap elements the draggable will snap to. Ignored if the snap option is false. Possible values: "inner", "outer", "both".

+ createShape() - calls the makeShapes() for every shapes.
+ makeshapes() - adds the div element with div element with block shape.

though the app is seems to difficult but draggable() in jquery UI made easy.

{%highlight javascript%}
(function(){
	createShape();

	$('.block').draggable({
		containment:'window',
		stack:'.block',
		snap:true,
		snapMode:'outer',
		snapTolerance:13
	});
	function makeshapes(shape){
		var i=0;
		for(i=0;i<8;i++){
			console.log(shape);
		$('#toolbox').append('<div class="block '+shape+'" </div>');
		}
	}

	function createShape(){
		var shapes = ['square', 'rectup', 'rectup long', 
					  'rectdown', 'triup', 'trileft', 'triright',
					  'paraleft', 'pararight', 'circle', 
					  'semitop', 'quartleft', 'quartright']
		var i = 0;
		for (i=0;i<13;i++){
			shape = shapes[i];
			makeshapes(shape);
		}
	}

	$('#toolbox').on('mousedown',function(){
		$('#instruction').fadeOut('slow');
	})
})();

{%endhighlight%}

CSS File below:

{%highlight ruby%}
body {
    font-family: Garamond, Baskerville, "Baskerville Old Face", "Hoefler Text", "Times New Roman", serif; 
    letter-spacing: 2px;
}
.block {
	position: absolute;
	cursor: pointer;
}
#container {
	text-align: right;
	padding: 15px;
}
#toolbox {
	position: relative;
	display: inline-block;
	height: 565px;
	width: 150px;
	margin: 0 40px;
}

#canvas {
	display: inline-block;
	height: 565px;
	width: 700px;
	text-align: center;
	vertical-align: top;
}

#instruction {
	margin-top: 150px; 
	margin-left: 100px;
	font-size: 24px;
	color: #BCB9D4;
}

h1{
	font-size: 32px;
	line-height: 3;
}

.square {
	height: 60px;
	width: 60px;
	top: 155px;
	left: 10px;
	background-color: #2E70FF;
}

.rectup {
	height: 70px;
	width: 40px;
	top: 295px;
	left: 15px;
	background-color: #6A4A3C;
}

.rectup.long {
	height: 120px;
	width: 30px;
	top: 85px;
	left: 105px;
	background-color: #CC333F;
}

.rectdown {
	height: 30px;
	width: 120px;
	top: 240px;
	left: 15px;
	background-color: #EB6841;
}

.triup {
	top: 75px;
	left: 0px;
	width: 0;
	height: 0;
	border-bottom: 50px solid #EDC951;
	border-left: 40px solid transparent;
	border-right: 40px solid transparent;
}

.trileft {
	top: 0;
	left: 90px;
	width: 0;
	height: 0;
	border-bottom: 60px solid #7F9D39;
	border-right: 60px solid transparent;
}

.triright {
	top: 0;
	left: 0;
	width: 0;
	height: 0;
	border-bottom: 60px solid #676690;
	border-left: 60px solid transparent;
}

.paraleft {
	top: 455px;
	left: 80px;
	width: 60px;
	height: 40px;
	-webkit-transform: skew(25deg);
	   -moz-transform: skew(25deg);
	     -o-transform: skew(25deg);
	background-color: #D48317;
}

.pararight {
	top: 525px;
	left: 80px;
	width: 60px;
	height: 40px;
	-webkit-transform: skew(-25deg);
	   -moz-transform: skew(-25deg);
	     -o-transform: skew(-25deg);
	background-color: #452159;
}

.circle {
	top: 305px;
	left: 85px;
	height: 50px;
	width: 50px;
	border-radius: 50%;
	background-color: #00A0B0;
}

.semitop {
	top: 385px;
	left: 35px;
	height: 40px;
	width: 80px;
	border-radius: 80px 80px 0 0;
	background-color: #3B45A6;
}

.quartleft {
	top: 455px;
	left: 5px;
	height: 40px;
	width: 40px;
	border-radius: 80px 0 0 0;
	background-color: #FB5A69;
}

.quartright {
	top: 525px;
	left: 5px;
	height: 40px;
	width: 40px;
	border-radius: 0 80px 0 0;
	background-color: #5CD465;
}


.clear {
	clear: both;
}

{%endhighlight%}
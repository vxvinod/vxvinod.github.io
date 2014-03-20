---
layout: post
title:  "mouse catcher is scarry but mind-blowing"
date:   2014-03-20
desc: "Today as a javascript oath i have created a mouse catcher application. where some couple of circles with various sizes will follow werever you move the mouse.in this i have learnt how to make the objects to move towards posinter,how to generate dynamic html"
---


Today as a javascript oath i have created a mouse catcher application. where some couple of circles with various sizes will follow werever you move the mouse.in this i have learnt how to make the objects to move towards posinter,how to generate dynamic html.

Explanation:

+ generate dynamic div from id from spot1 to spot20,and append it to container div.
+ get the present mouse pointer x and y coordinates.
+ manipulate the position of those dynamic div elements using present x,y.
	xp =  presentx-xp / speed (speed to make the object to move slower).
+ if i fail to use speed then object will stick to the mouse pointer.

github link: https://github.com/vxvinod/javascript-oath/tree/master/exercise-7-mouse-catcher


Javascript file below

{%highlight javascript%}

(function(){

	generateSpots(21);

	var mousex=0,mousey=0;

	function generateSpots(value){
		for(var i=0;i<value;i++){
		var size = spotSize(3,65);
		var color = spotColor();
		//alert(size);
		//alert(color);
		$('#container').append('<div class="spot" id="spot'+i+'"></div>');
		$('#spot'+i).css({backgroundColor:color,height:size,width:size});
		}
		for(var i=0;i<21;i++){
		moveSpots('#spot'+i,spotSize(8,50));
	}
	};

	function spotSize(min,max){
		return Math.floor(Math.random()*(max-min+1)+min);
	};

	function spotColor(){
		return '#'+Math.random().toString(16).slice(2,8).toUpperCase();
	};
//generate x,y 
	$(document).on('mousemove',function(e){		
		mousex = e.pageX;
		mousey = e.pageY;
	});
	

	function moveSpots(elm,speed){
		var xp = 0, yp = 0;
		var loop = setInterval(function () {
			xp += (mousex - xp) / speed;
			yp += (mousey - yp) / speed;
			$(elm).css({left:xp, top:yp});
		}, 30);
	};

})();

{%endhighlight%}



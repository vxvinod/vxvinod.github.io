---
layout: post
title:  "Mr Js I am here to screw you in to my head"
date:   2014-01-31 
desc: "one day one javascript example will enrich my javascript knowledge start the game and fail and fail and fail to learn"
---


###Amazing Animation by CSS that i trigger thriugh Javascript its damn awesome.

 
 Today i will take a Oath that i will try one example of javscript daily ,i dont care if it is damn simple so that i can move from beginner level to expert level.Practice makes a man perfect and i believe in it . i will do it i knew that what is going on your mind what this idotic guys is keeps on talking but no action has been done.ok chalo lets start the game.

 Today i am going to take one exercise from my guru Jennifer Dawalt that i will animate the images that i click and animation i will be using are 'shake''hop','spin','grow','hooray'. these animation css i downloaded from internet.bere my aim is to master in javascript so i will concentrate on Mr Js.

 first we will look at html that i have included six images of animal and its below.

{% highlight javascript%}
 <!DOCTYPE html>
<html lang>
<head>
  <meta charset="utf-8">
  <title>Animals | Sivaji the Boss</title>
  <link rel="stylesheet" href="jiggler.css">
  <script src="http://ajax.googleapis.com/ajax/libs/jquery/1.9.1/jquery.min.js"></script>

 </head>

<body>
    <div id ="wrapper">
		<div id ="container">
		<div id="dog" class="animal-box"><img src = "dog.jpg"></div>
		<div id="lion" class="animal-box"><img src = "lion.jpg"></div>
		<div id="horse" class="animal-box"><img src = "horse.jpg"></div>
		<div id="panda" class="animal-box"><img src = "panda.jpg"></div>
		<div id="tiger" class="animal-box"><img src = "tiger.jpg"></div>
		<div id="monkey" class="animal-box"><img src = "monkey.jpg"></div>
	</div>

	</div>


</body>
  <script type="text/javascript" src="jiggler.js"></script>
</html>

{%endhighlight%}


CSS is too big so i will include at end . but as now we will move on to Mr JS

####Action To Be Done:
1.there will be 6 images on page, when we click on it randomly some action should happen.
2.these animation will be taken care by CSS.
3.when user clicks on one image in a page that class name of animation such as 'shaky','hop'...should get added as an attribute in to the tag. this is done by javascript.
4.once class name attribute is inserted then animation will be happened.now how long the animation will happen.
5.in order to stop the animation we need to remove the class inserted.
6.removing the class is done by removeClass() but this to be done after 2 seconds.
7. this can be acheived using setTimout()

setTimeout(function(){alert("Hello")},3000);

The setTimeout() method calls a function or evaluates an expression after a specified number of milliseconds.

+ 1000 ms = 1 second.

+ The function is only executed once. If you need to repeat execution, use the setInterval() method.

+ Use the clearTimeout() method to prevent the function to run.

Mr JS its your time

{%highlight javascript%}
(function (){
	var animation =['shake','hop','spin','grow','hooray'];

	 function getRandomNumber(){
		var rand = Math.floor((Math.random()*4)+1);
		//alert(rand);
		return rand
	}
	$('.animal-box').on('click',function(){

		var animal = this ;
		var anim = animation[getRandomNumber()];
		$(animal).addClass(anim);

		setTimeout(function(){
			$(animal).removeClass(anim);
		},2000);
	})
})();
{%endhighlight%}
####Explanation:
1.once the image is clicked by the user randomnly animnation should happen.
2.$('.animal-box') will contain all animals images so animal image that is clicked by the user will bein 'this' keyword that is assigned to var animal.
3.we have created a array of animation =['shake','hop','spin','grow','hooray'].
4.we are adding class to the image that is clicked using addClass() function by generating randomnumber and fetching the data from animation .
5.once class name is added necessary action will be taken by CSS.
6.In order to stop the animation we need to remove the class but after 2 seconds. 
7.so i used setTimeout() function.where after 2 seconds the function inside the setTimeout will be executed.
8.that is all to be done.


Css i have used is below
{%highlight css%}
#wrapper{
	width:900px;	
	padding-top: 50px;
	padding-bottom: 50px;
	margin:0 auto;
}

#container{
	height:600px;
	width:900px;
	position:relative;
}


#dog{
	bottom: 0;
	right: 20px;
}

#horse {
	top: 50px;
	left: 50px;
}

#monkey {
	top: 60px;
	left: 720px;
}

#panda {
	top: 270px;
	left: 50px;
}

#tiger {
	top: 200px;
	left: 380px;
}

#lion {
	top: 50px;
	left: 350px;
}

.animal-box{
	position:absolute;
}

.shake {
	-webkit-animation: shake 1s 2;
	-moz-animation: shake 1s 2;
	-ms-animation: shake 1s 2;
	animation: shake 1s 2;
}
.hop {
	-webkit-animation: hop 2s;
	-moz-animation: hop 2s;
	-ms-animation: hop 2s;
	animation: hop 2s;
}
.spin {
	-webkit-animation: spin 2s;
	-moz-animation: spin 2s;
	-ms-animation: spin 2s;
	animation: spin 2s;
}
.grow {
	z-index: 30;
	-webkit-animation: grow 2s;
	-moz-animation: grow 2s;
	-ms-animation: grow 2s;
	animation: grow 2s;
}
.hooray {
	z-index: 20;
	-webkit-animation: hooray 2s;
	-moz-animation: hooray 2s;
	-ms-animation: hooray 2s;
	animation: hooray 2s;
}

/* Webkit Animations */

@-webkit-keyframes shake {
	0%, 100% {-webkit-transform: translateX(0);}
	5%, 15%, 25%, 35%, 45%, 55%, 65%, 75%, 85%, 95% {-webkit-transform: translateX(-20px);}
	10%, 20%, 30%, 40%, 50%, 60%, 70%, 80%, 90% {-webkit-transform: translateX(20px);}
}

@-webkit-keyframes hop {
	0% {-webkit-transform: translateX(0);}
	12% {-webkit-transform: translateY(-100px) rotate(-20deg);}
	25% {-webkit-transform: translateX(-20px);}
	37% {-webkit-transform: translateY(-100px) rotate(20deg);}
	50% {-webkit-transform: translateX(20px);}
	62% {-webkit-transform: translateY(-100px) rotate(-20deg);}
	75% {-webkit-transform: translateX(-20px);}
	87% {-webkit-transform: translateY(-100px) rotate(20deg);}
	100% {-webkit-transform: translateX(0);}
}

@-webkit-keyframes spin {
	0% {-webkit-transform: rotate(0deg)}
	8% {-webkit-transform: rotate(-50deg)}
	12% {-webkit-transform: rotate(-50deg) }
	100% {-webkit-transform: rotate(3600deg)}
}

@-webkit-keyframes grow {
	0% { -webkit-transform: scale(1); }
	75% { -webkit-transform: scale(2.5); }
	90% { -webkit-transform: scale(0.6); }
	93% { -webkit-transform: scale(1.4); }
	95% { -webkit-transform: scale(0.8); }
	97% { -webkit-transform: scale(1.2); }
	100% { -webkit-transform: scale(1); }
}

@-webkit-keyframes hooray {
	0% {-webkit-transform: translateX(0) translateY(0) scale(1, 1);}
	10% {-webkit-transform: translateX(60px) translateY(-60px) scale(1.5, 1.5);}
	20% {-webkit-transform: translateX(0) translateY(0) scale(.7, .7);}
	30% {-webkit-transform: translateX(-60px) translateY(-60px) scale(1.5, 1.5);}
	40% {-webkit-transform: translateX(0) translateY(0) scale(.7, .7);}
	50% {-webkit-transform: translateX(60px) translateY(-60px) scale(1.5, 1.5);}
	60% {-webkit-transform: translateX(0) translateY(0) scale(.7, .7);}
	70% {-webkit-transform: translateX(-60px) translateY(-60px) scale(1.5, 1.5);}	
	80% {-webkit-transform: translateX(0) translateY(0) scale(.7, .7);}
	90% {-webkit-transform: translateX(60px) translateY(-60px) scale(1.5, 1.5);}
	100% {-webkit-transform: translateX(0) translateY(0) scale(1, 1);}
}

/* Moz Animations */

@-moz-keyframes shake {
	0%, 100% {-moz-transform: translateX(0);}
	5%, 15%, 25%, 35%, 45%, 55%, 65%, 75%, 85%, 95% {-moz-transform: translateX(-20px);}
	10%, 20%, 30%, 40%, 50%, 60%, 70%, 80%, 90% {-moz-transform: translateX(20px);}
}

@-moz-keyframes hop {
	0% {-moz-transform: translateX(0);}
	12% {-moz-transform: translateY(-100px) rotate(-20deg);}
	25% {-moz-transform: translateX(-20px);}
	37% {-moz-transform: translateY(-100px) rotate(20deg);}
	50% {-moz-transform: translateX(20px);}
	62% {-moz-transform: translateY(-100px) rotate(-20deg);}
	75% {-moz-transform: translateX(-20px);}
	87% {-moz-transform: translateY(-100px) rotate(20deg);}
	100% {-moz-transform: translateX(0);}
}

@-moz-keyframes spin {
	0% {-moz-transform: rotate(0deg)}
	8% {-moz-transform: rotate(-50deg)}
	12% {-moz-transform: rotate(-50deg) }
	100% {-moz-transform: rotate(3600deg)}
}

@-moz-keyframes grow {
	0% { -moz-transform: scale(1); }
	75% { -moz-transform: scale(2.5); }
	90% { -moz-transform: scale(0.6); }
	93% { -moz-transform: scale(1.4); }
	95% { -moz-transform: scale(0.8); }
	97% { -moz-transform: scale(1.2); }
	100% { -moz-transform: scale(1); }
}

@-moz-keyframes hooray {
	0% {-moz-transform: translateX(0) translateY(0) scale(1, 1);}
	10% {-moz-transform: translateX(60px) translateY(-60px) scale(1.5, 1.5);}
	20% {-moz-transform: translateX(0) translateY(0) scale(.7, .7);}
	30% {-moz-transform: translateX(-60px) translateY(-60px) scale(1.5, 1.5);}
	40% {-moz-transform: translateX(0) translateY(0) scale(.7, .7);}
	50% {-moz-transform: translateX(60px) translateY(-60px) scale(1.5, 1.5);}
	60% {-moz-transform: translateX(0) translateY(0) scale(.7, .7);}
	70% {-moz-transform: translateX(-60px) translateY(-60px) scale(1.5, 1.5);}	
	80% {-moz-transform: translateX(0) translateY(0) scale(.7, .7);}
	90% {-moz-transform: translateX(60px) translateY(-60px) scale(1.5, 1.5);}
	100% {-moz-transform: translateX(0) translateY(0) scale(1, 1);}
}

/* Opera Animations */

@-o-keyframes shake {
	0%, 100% {-o-transform: translateX(0);}
	5%, 15%, 25%, 35%, 45%, 55%, 65%, 75%, 85%, 95% {-o-transform: translateX(-20px);}
	10%, 20%, 30%, 40%, 50%, 60%, 70%, 80%, 90% {-o-transform: translateX(20px);}
}

@-o-keyframes hop {
	0% {-o-transform: translateX(0);}
	12% {-o-transform: translateY(-100px) rotate(-20deg);}
	25% {-o-transform: translateX(-20px);}
	37% {-o-transform: translateY(-100px) rotate(20deg);}
	50% {-o-transform: translateX(20px);}
	62% {-o-transform: translateY(-100px) rotate(-20deg);}
	75% {-o-transform: translateX(-20px);}
	87% {-o-transform: translateY(-100px) rotate(20deg);}
	100% {-o-transform: translateX(0);}
}

@-o-keyframes spin {
	0% {-o-transform: rotate(0deg)}
	8% {-o-transform: rotate(-50deg)}
	12% {-o-transform: rotate(-50deg) }
	100% {-o-transform: rotate(3600deg)}
}

@-o-keyframes grow {
	0% { -o-transform: scale(1); }
	75% { -o-transform: scale(2.5); }
	90% { -o-transform: scale(0.6); }
	93% { -o-transform: scale(1.4); }
	95% { -o-transform: scale(0.8); }
	97% { -o-transform: scale(1.2); }
	100% { -o-transform: scale(1); }
}

@-o-keyframes hooray {
	0% {-o-transform: translateX(0) translateY(0) scale(1, 1);}
	10% {-o-transform: translateX(60px) translateY(-60px) scale(1.5, 1.5);}
	20% {-o-transform: translateX(0) translateY(0) scale(.7, .7);}
	30% {-o-transform: translateX(-60px) translateY(-60px) scale(1.5, 1.5);}
	40% {-o-transform: translateX(0) translateY(0) scale(.7, .7);}
	50% {-o-transform: translateX(60px) translateY(-60px) scale(1.5, 1.5);}
	60% {-o-transform: translateX(0) translateY(0) scale(.7, .7);}
	70% {-o-transform: translateX(-60px) translateY(-60px) scale(1.5, 1.5);}	
	80% {-o-transform: translateX(0) translateY(0) scale(.7, .7);}
	90% {-o-transform: translateX(60px) translateY(-60px) scale(1.5, 1.5);}
	100% {-o-transform: translateX(0) translateY(0) scale(1, 1);}
}

/* MS Animations */

@-ms-keyframes shake {
	0%, 100% {-ms-transform: translateX(0);}
	5%, 15%, 25%, 35%, 45%, 55%, 65%, 75%, 85%, 95% {-ms-transform: translateX(-20px);}
	10%, 20%, 30%, 40%, 50%, 60%, 70%, 80%, 90% {-ms-transform: translateX(20px);}

@-ms-keyframes hop {
	0% {-ms-transform: translateX(0);}
	12% {-ms-transform: translateY(-100px) rotate(-20deg);}
	25% {-ms-transform: translateX(-20px);}
	37% {-ms-transform: translateY(-100px) rotate(20deg);}
	50% {-ms-transform: translateX(20px);}
	62% {-ms-transform: translateY(-100px) rotate(-20deg);}
	75% {-ms-transform: translateX(-20px);}
	87% {-ms-transform: translateY(-100px) rotate(20deg);}
	100% {-ms-transform: translateX(0);}
}

@-ms-keyframes spin {
	0% {-ms-transform: rotate(0deg)}
	8% {-ms-transform: rotate(-50deg)}
	12% {-ms-transform: rotate(-50deg) }
	100% {-ms-transform: rotate(3600deg)}
}

@-ms-keyframes grow {
	0% { -ms-transform: scale(1); }
	75% { -ms-transform: scale(2.5); }
	90% { -ms-transform: scale(0.6); }
	93% { -ms-transform: scale(1.4); }
	95% { -ms-transform: scale(0.8); }
	97% { -ms-transform: scale(1.2); }
	100% { -ms-transform: scale(1); }
}

@-ms-keyframes hooray {
	0% {-ms-transform: translateX(0) translateY(0) scale(1, 1);}
	10% {-ms-transform: translateX(60px) translateY(-60px) scale(1.5, 1.5);}
	20% {-ms-transform: translateX(0) translateY(0) scale(.7, .7);}
	30% {-ms-transform: translateX(-60px) translateY(-60px) scale(1.5, 1.5);}
	40% {-ms-transform: translateX(0) translateY(0) scale(.7, .7);}
	50% {-ms-transform: translateX(60px) translateY(-60px) scale(1.5, 1.5);}
	60% {-ms-transform: translateX(0) translateY(0) scale(.7, .7);}
	70% {-ms-transform: translateX(-60px) translateY(-60px) scale(1.5, 1.5);}	
	80% {-ms-transform: translateX(0) translateY(0) scale(.7, .7);}
	90% {-ms-transform: translateX(60px) translateY(-60px) scale(1.5, 1.5);}
	100% {-ms-transform: translateX(0) translateY(0) scale(1, 1);}
}

/* Arbitrary Animations */

@keyframes shake {
	0%, 100% {transform: translateX(0);}
	5%, 15%, 25%, 35%, 45%, 55%, 65%, 75%, 85%, 95% {transform: translateX(-20px);}
	10%, 20%, 30%, 40%, 50%, 60%, 70%, 80%, 90% {transform: translateX(20px);}
}

@keyframes hop {
	0% {transform: translateX(0);}
	12% {transform: translateY(-100px) rotate(-20deg);}
	25% {transform: translateX(-20px);}
	37% {transform: translateY(-100px) rotate(20deg);}
	50% {transform: translateX(20px);}
	62% {transform: translateY(-100px) rotate(-20deg);}
	75% {transform: translateX(-20px);}
	87% {transform: translateY(-100px) rotate(20deg);}
	100% {transform: translateX(0);}
}

@keyframes spin {
	0% {transform: rotate(0deg)}
	8% {transform: rotate(-50deg)}
	12% {transform: rotate(-50deg) }
	100% {transform: rotate(3600deg)}
}

@keyframes grow {
    0% { transform: scale(1); }
	75% { transform: scale(2.5); }
	90% { transform: scale(0.6); }
	93% { transform: scale(1.4); }
	95% { transform: scale(0.8); }
	97% { transform: scale(1.2); }
	100% { transform: scale(1); }
}

@keyframes hooray {
	0% {transform: translateX(0) translateY(0) scale(1, 1);}
	10% {transform: translateX(60px) translateY(-60px) scale(1.5, 1.5);}
	20% {transform: translateX(0) translateY(0) scale(.7, .7);}
	30% {transform: translateX(-60px) translateY(-60px) scale(1.5, 1.5);}
	40% {transform: translateX(0) translateY(0) scale(.7, .7);}
	50% {transform: translateX(60px) translateY(-60px) scale(1.5, 1.5);}
	60% {transform: translateX(0) translateY(0) scale(.7, .7);}
	70% {transform: translateX(-60px) translateY(-60px) scale(1.5, 1.5);}	
	80% {transform: translateX(0) translateY(0) scale(.7, .7);}
	90% {transform: translateX(60px) translateY(-60px) scale(1.5, 1.5);}
	100% {transform: translateX(0) translateY(0) scale(1, 1);}
}



{%endhighlight%}
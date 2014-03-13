---
layout: post
title:  "Third day JS makes my mind cool dice roller great"
date:   2014-03-13 
desc: "indru(Today in tamil) we will see the third man stay of javascript practice .I dont wanna elaborate bored story daily in order to fill my blog.i will try to be crisp and short ."
---


indru(Today in tamil) we will see the third man stay of javascript practice .I dont wanna elaborate bored story daily in order to fill my blog.i will try to be crisp and short .

redarding todays exercise today we will design a roller dice so that when user clicks a button two dice should get rolled of and sum of those numbers should get displayed. though it seems to be easy the work invollved is easy but more to use our brain.

###Plan To Execute the dice roller game.

+ rollTheDice() function with one argument,where random number is generated.
+ There will be 7 dots in a dice.to display the dots as per the random number generated.
+ in 7 dots ,3 dots are arranged vertically on left of dice,1 dot at center,3 dots at the right of the dice vertically.
+ if the random number is 1, all dots => hided ,only center 4th dot =>shown.
+ if the random number is 2, all dots => hided ,only 1st and 7th dot =>shown.
+ if the random number is 3, all dots => hided ,only 2nd,4th,6th dot =>shown.
+ if the random number is 4, all dots => shown ,only 2nd,4th,6th dot =>hided.
+ if the random number is 5, all dots => shown ,only 2nd and 6th dot =>hided.
+ if the random number is 6, all dots => shown ,only center 4th dot =>hided.

###To introduce shadow effect to button

+ when a button is mouse down then a particular class is added where shadow is introduced.

####    $(".button").removeClass("btn_up"); 
####        $(".button").addClass("btn_down");
###Shake animation is introduced

+ when button is clicked, dice and text should gets shaked.
+ so we added shake class when button is clicked.note that particular class should be removed.

####	$('h1').addClass('shake'); //to introduce shake animation we add it to h1
####$('.die_face').addClass('shake'); //introducing shake animation to diece.



{%highlight javascript%}
(function(){
	$('.pip_4').hide(); // we are using seven dots to represent numbers ,to hide middle dot in a die
	buttonPress();

	function buttonPress(){
		$(".button").on("mousedown",function(){
			console.log("mousedown");
			//to introduce button effect - refer CSS for shadow
			$(".button").removeClass("btn_up"); 
			$(".button").addClass("btn_down");
		});

		$(".button").on("mouseup",function(){
			console.log("mouseup");
			//to introduce button effect - refer CSS for shadow
			$(".button").removeClass("btn_down");
			$(".button").addClass("btn_up");
		});
	} 

	$(".button").on("click",function(){
		$('h1').text("shake shake shake.....");
		$('h1').addClass('shake'); //to introduce shake animation we add it to h1
		$('.die_face').addClass('shake'); //introducing shake animation to diece.

		setTimeout(function(){
			var roll1 = rollTheDice('#die_1');//rolling the first die
			var roll2 = rollTheDice('#die_2'); //rolling the second die
			console.log(roll1+roll2); 
			$('h1').text(roll1+roll2); // apeending result of rolled diece.
			$('h1').removeClass('shake'); //removing the shake animation
			$('.die_face').removeClass('shake');
		},500);

		function rollTheDice(die){
			var roll = Math.floor((Math.random()*6)+1); // creating random numberes for dies
			//alert(roll);
			//assigning each dot in a die to variable so that it can be used to hide and shoe whenever needed
			var all = die+' .pip'; 
			var pip_1 = die+'_pip_1';
			var pip_2 = die+'_pip_2';
			var pip_3 = die+'_pip_3';
			var pip_4 = die+'_pip_4';
			var pip_5 = die+'_pip_5';
			var pip_6 = die+'_pip_6';
			var pip_7 = die+'_pip_7';

			if(roll==1){
				$(all).hide(); //if you get 1 ,hide all and make center dot alone visible.
				$(pip_4).show();
			}

			if(roll==2){
				//if you get 2 ,hide all and display 2nd and 6th dot.
				$(all).hide();
				$(pip_2+','+pip_6).show();
			}
				//if you get 3,hide all and display first,fourth and seventh dot alone.
			if(roll==3){
				$(all).hide();
				$(pip_1+','+pip_4+','+pip_7).show();
			}

			if(roll==4){
				//if you get 4,show all and hide second and fourth dot alone.
				$(all).show();
				$(pip_2+','+pip_4+','+pip_6).hide();
			}

			if(roll==5){
				//if you get 4,show all and hide second and sixth dot alone.
				$(all).show();
				$(pip_2+','+pip_6).hide();
			}

			if(roll==6){
				//if you get 4,show all and hide fourth dot alone.
				$(all).show();
				$(pip_4).hide();
			}
			return roll;

		}
	});

})();
{%endhighlight%}

####and the css is here:

{%highlight css%}
body {
	background: #F5F5DC;
	text-align: center;
	font-family: "Arial Black", "Arial Bold", Gadget, sans-serif;
	letter-spacing: 2px;
	color: #1d113b;
	overflow: scroll;
}

.container {
	padding-bottom: 34px;
}

#title {
	width: 800px;
	padding: 50px 0 50px;
	margin: 0 auto;
	font-size: 80px;
	text-shadow: -1px -1px 0 rgba(0,0,0,0.2),
				  1px 1px 0 rgba(255,255,255,0.5);	
}

h1 {
	;
}

.shake {
	-webkit-animation: shake 1s;
	-moz-animation: shake 1s;
	-ms-animation: shake 1s;
	animation: shake 1s;
}

.button {
	width: 200px;
	margin: 50px auto;
	padding: 15px  5px;
	font-size: 24px;
	background-color: #46d11d;
	cursor: pointer;
	border-radius: 5px;
}

.btn_up {
	box-shadow: 10px 5px 5px #888888;   
}

.btn_down {
box-shadow: 2px 5px 5px #888888;
}

.die_face {
	position: relative;
	display: inline-block;
	height: 150px;
	width: 150px;
	background: #3300cc;
	margin: 50px 20px;
	border-radius: 5px;

	box-shadow: 1px 1px 3px rgba(0,0,0,0.2) inset, 
				-1px -1px 3px rgba(0,0,0,0.2) inset,
				-1px -1px 0 rgba(255,255,255,0.2);

}

.pip {
	position: absolute;
	height: 33px;
	width: 33px;
	margin: 9px;
	border-radius: 50%;
	box-shadow: 1px 1px 0 rgba(0,0,0,0.7) inset, 
				1px 1px 0 rgba(255,255,255,0.5);

	background-color: #fff;
	background: -webkit-gradient(radial, center center, 0, center center, 460, from(#fff), to(#c1bec9));
	background: -webkit-radial-gradient(circle, #fff, #c1bec9);
	background: -moz-radial-gradient(circle, #fff, #c1bec9);
  	background: -ms-radial-gradient(circle, #fff, #c1bec9);
}

/* pip order [1   5
 		      2 4 6
 		      3   7]*/

.pip_1 {
	top: 3px;
	left: 3px;
}

.pip_2 {
	top: 50px;
	left: 3px;
}

.pip_3 {
	top: 97px;
	left: 3px;
}

.pip_4 {
	top: 50px;
	left: 50px;
}

.pip_5 {
	top: 3px;
	right: 3px;
}

.pip_6 {
	right: 3px;
	top: 50px;
}

.pip_7{
	right: 3px;
	top: 97px;
}

footer {
	position: absolute;
	bottom: 0;
	left: 0;	
	right: 0;
	height: 34px;
	background-color: #fe2f44;
	border-top: 1px solid #fe162d;
}

#links_container {
	padding: 10px;
	font-family: Arial, "Helvetica Neue", Helvetica, sans-serif;
	font-size: 14px;
}

#links_container a {
	color: #F5F5DC;
	margin: 10px;
	text-decoration: none;
}

#links_container a:visited {
	color: #F5F5DC;
}

#links_container a:hover {
	color: #46d11d;
}

@-webkit-keyframes shake {
	0%, 100% {-webkit-transform: translateX(0);} 
	5%, 15%, 25%, 35%, 45%, 55%, 65%, 75%, 85%, 95% {-webkit-transform: translateX(5px);} 
	10%, 20%, 30%, 40%, 50%, 60%, 70%, 80%, 90% {-webkit-transform: translateX(-5px);} 
}

@-moz-keyframes shake {
	0%, 100% {-moz-transform: translateX(0);} 
	5%, 15%, 25%, 35%, 45%, 55%, 65%, 75%, 85%, 95% {-moz-transform: translateX(5px);} 
	10%, 20%, 30%, 40%, 50%, 60%, 70%, 80%, 90% {-moz-transform: translateX(-5px);} 
}

@-o-keyframes shake {
	0%, 100% {-o-transform: translateX(0);} 
	5%, 15%, 25%, 35%, 45%, 55%, 65%, 75%, 85%, 95% {-o-transform: translateX(5px);} 
	10%, 20%, 30%, 40%, 50%, 60%, 70%, 80%, 90% {-o-transform: translateX(-5px);} 
}

@-ms-keyframes shake {
	0%, 100% {-ms-transform: translateX(0);} 
	5%, 15%, 25%, 35%, 45%, 55%, 65%, 75%, 85%, 95% {-ms-transform: translateX(5px);} 
	10%, 20%, 30%, 40%, 50%, 60%, 70%, 80%, 90% {-ms-transform: translateX(-5px);} 
}

@keyframes shake {
	0%, 100% {transform: translateX(0);} 
	5%, 15%, 25%, 35%, 45%, 55%, 65%, 75%, 85%, 95% {transform: translateX(5px);} 
	10%, 20%, 30%, 40%, 50%, 60%, 70%, 80%, 90% {transform: translateX(-5px);} 
}
{%endhighlight%}

####HTML is below
{%highlight html%}
<!DOCTYPE html>
<html lang>
<head>
  <meta charset="utf-8">
  <title>roll the dice | help Wekare</title>
   <link rel="stylesheet" href="reset.css">
  <link rel="stylesheet" href="roll_dice.css">
  <script src="http://ajax.googleapis.com/ajax/libs/jquery/1.9.1/jquery.min.js"></script>

 </head>

<body>
	<div class = "container">

		<div class = "title">
			<h2>Give a roll and have fun!!!!!!!</h2><br>
			<h1></h1>
		</div>

		<div class = "dice_container">
			<div class ="die_face" id = "die_1">
				<div class="pip pip_1" id="die_1_pip_1"></div>
				<div class="pip pip_2" id="die_1_pip_2"></div>
				<div class="pip pip_3" id="die_1_pip_3"></div>
				<div class="pip pip_4" id="die_1_pip_4"></div>
				<div class="pip pip_5" id="die_1_pip_5"></div>
				<div class="pip pip_6" id="die_1_pip_6"></div>
				<div class="pip pip_7" id="die_1_pip_7"></div>
			</div>
			<div class ="die_face" id = "die_2">
				<div class="pip pip_1" id="die_2_pip_1"></div>
				<div class="pip pip_2" id="die_2_pip_2"></div>
				<div class="pip pip_3" id="die_2_pip_3"></div>
				<div class="pip pip_4" id="die_2_pip_4"></div>
				<div class="pip pip_5" id="die_2_pip_5"></div>
				<div class="pip pip_6" id="die_2_pip_6"></div>
				<div class="pip pip_7" id="die_2_pip_7"></div>
			</div>

		</div>


		<div class = "button btn_up" id="shaker">
			shakE Me
		</div>

	</div>

</body>
  <script type="text/javascript" src="roll_dice.js"></script>
</html>
{%endhighlight%}
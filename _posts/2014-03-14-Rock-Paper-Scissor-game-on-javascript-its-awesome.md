---
layout: post
title:  "Rock Paper Scissor game on javascript its awesome"
date:   2014-03-14
desc: "successfully on fourth day of javascript journey, today we will play Rock,paper,scissors.you might think what this idiot said gonna explain javascript but now he is going to play RPS.cool down readies we are gonna develop Rock paper Scissor game using javascript.yes its amazing right yup lets start the ride........."
---




successfully on fourth day of javascript journey, today we will play Rock,paper,scissors.you might think what this idiot said gonna explain javascript but now he is going to play RPS.cool down readies we are gonna develop Rock paper Scissor game using javascript.yes its amazing right yup lets start the ride.........

###Plan for Execution:

+ We need to do three actions to design the game.
    - get the input from user.
	- display counter as 3...2...1...
	- display the result.

+ for user we will be getting the input via (this.id).user will be clicking the button from the id attribute we can get the input.
+ for computer choice we will be generating random numbers.
+ next counter function counts the action when user clicks the button ,it gives an effect as 3..2..1..
+ third function is manipulation the results ,here we are showing the respective image based on the choices ,if user choice is rock then rock image will be displayes ,this comes true for computer also.
+ then winning and loosing is decided based on 

if (user_choice== comp_choice){
			$('#result').text("TIE"); //if both are same result is tie
		}else if((user_choice=='scissor' && comp_choice=='paper')||(user_choice=='paper' && comp_choice=='rock')||(user_choice=='rock' && comp_choice=='scissor')) {
			$('#result').text("You Win"); //you win
		}else {
			$('#result').text("You Lose");
		}


{%highlight javascript%}
(function(){

var choices=['rock','paper','scissor']; //collect items in a array
var user_choice;
var comp_choice;
$('.but').on('click',getInput); 

function getInput(){
	user_choice = this.id; //getting user choice from attribute
	comp_choice= choices[Math.floor(Math.random()*3)]; //getting computer choice
	console.log(user_choice);
	console.log(comp_choice);

	$('.left').hide(); //hiding the items
	$('.right').hide(); //hiding the items
	$('.hand').show(); //showing the hand

	countTimer();
}

function countTimer(){
	$('.player').addClass('shake');
	var count=3; //initializing the counter
	counter();

	function counter(){
		$('#result').text(""); 
		$('#result').text(count); //displaying like timer
		console.log(count);
		count=count-1;

		if(count==0){
			displayResult(); 
		}else{
			setTimeout(counter,500);
		}
	}
}

function displayResult(){
	$('.player').removeClass('shake');
	$('.hand').hide();
	$('#left-'+comp_choice).show(); //display the computer choice image
	$('#right-'+ user_choice).show();//display the user choice image

	setTimeout(function(){
		if (user_choice== comp_choice){
			$('#result').text("TIE"); //if both are same result is tie
		}else if((user_choice=='scissor' && comp_choice=='paper')||(user_choice=='paper' && comp_choice=='rock')||(user_choice=='rock' && comp_choice=='scissor')) {
			$('#result').text("You Win"); //you win
		}else {
			$('#result').text("You Lose");
		}
	},500);

}


})();
{%endhighlight%}

###HTML BELOW

{%highlight javascript%}
<!DOCTYPE html>
<html lang>
<head>
  <meta charset="utf-8">
  <title>Rock | Paper | Scissors</title>
   <link rel="stylesheet" href="reset.css">
  <link rel="stylesheet" href="rps.css">
  <script src="http://ajax.googleapis.com/ajax/libs/jquery/1.9.1/jquery.min.js"></script>

 </head>

<body>
	<div class = "container">

		<div class = "player" id="computer-container">
			<div class="title" id="comp_title">Computer</div>
			
			<div class="class-left">
				<div class="hand" id="left-hand"><img src="left_hand.jpeg"></div>
				<div class="left disp" id="left-rock" style="display:none"><img src="rock.jpeg"></div>
				<div class="left disp" id="left-paper" style="display:none"><img src="paper.jpeg"></div>
				<div class="left disp" id="left-scissor" style="display:none"><img src="scissors.jpeg"></div>
			</div>
		</div>
			<div id="result">choose your weapon</div>


		<div class = "player" id="player-container">
			<div class="title" id="player_title">You</div>
		
			<div class="class-right">
				<div class="hand" id="right-hand" ><img src="right_hand.jpeg"></div>
				<div class="right disp" id="right-rock" style="display:none"><img src="rock.jpeg"></div>
				<div class="right disp" id="right-paper" style="display:none"><img src="paper.jpeg"></div>
				<div class="right disp" id="right-scissor" style="display:none"><img src="scissors.jpeg"></div>
			</div>

		</div>

		<div class="button">
			<div class="but btn_up" id="rock">Rock</div>
			<div class="but btn_up" id="paper">Paper</div>
			<div class="but btn_up" id="scissor">Scissor</div>
		</div>
	</div>

</body>
  <script type="text/javascript" src="rps.js"></script>
</html>

{%endhighlight%}

####css below

{%highlight javascript%}
body {
     text-align: center;
     font-family: "Arial Rounded MT Bold", "Helvetica Rounded", Arial, sans-serif;
     font-size: 24px;
     letter-spacing: 2px;
     line-height: 1.3;
     color: #097054;
}

.container {
	width: 930px;
	margin: 0 auto;
    padding-bottom: 58px;
}


.player {
	position: relative;
	display: inline-block;
	width: 350px;
	height: 530px;
	vertical-align: top;
}

#result{
	display: inline-block;
	width: 200px;
	margin-top: 100px;
	color: #008f68;
	font-size: 32px;
}

.but{
	width:100px;
	padding:20px 0;
	color: blue;
	display:inline-block;
	cursor: pointer;
	background-color: orange;
	font-size: 15px;

}
.btn_up {
	box-shadow: 10px 5px 5px #888888;   
}

.btn_down {
box-shadow: 2px 5px 5px #888888;
}

.shake {
	-webkit-animation: shake 1.5s;
	-moz-animation: shake 1.5s;
	-ms-animation: shake 1.5s;
	animation: shake 1.5s;
}


@-webkit-keyframes shake {
	0%, 100% {-webkit-transform: translateY(0);} 
	14%, 43%, 71%, 95%, 100% {-webkit-transform: translateY(30px);
						 -webkit-animation-timing-function: ease-out; } 
	29%, 57%, 86% {-webkit-transform: translateY(-30px);
				   -webkit-animation-timing-function: ease-out} 
}
@-moz-keyframes shake {
	0%, 100% {-moz-transform: translateY(0);} 
	14%, 43%, 71%, 95%, 100% {-moz-transform: translateY(30px);
						 -moz-animation-timing-function: ease-out; } 
	29%, 57%, 86% {-moz-transform: translateY(-30px);
				   -moz-animation-timing-function: ease-out} 
}
@-o-keyframes shake {
	0%, 100% {-o-transform: translateY(0);} 
	14%, 43%, 71%, 95%, 100% {-o-transform: translateY(30px);
						 -o-animation-timing-function: ease-out; } 
	29%, 57%, 86% {-o-transform: translateY(-30px);
				   -o-animation-timing-function: ease-out} 
}
@-ms-keyframes shake {
	0%, 100% {-ms-transform: translateY(0);} 
	14%, 43%, 71%, 95%, 100% {-ms-transform: translateY(30px);
						 -ms-animation-timing-function: ease-out; } 
	29%, 57%, 86% {-ms-transform: translateY(-30px);
				   -ms-animation-timing-function: ease-out} 
}
@keyframes shake {
	0%, 100% {transform: translateY(0);} 
	14%, 43%, 71%, 95%, 100% {transform: translateY(30px);
						 animation-timing-function: ease-out; } 
	29%, 57%, 86% {transform: translateY(-30px);
				   animation-timing-function: ease-out} 
}


{%endhighlight%}
---
layout: post
title:  "change background color as per time every second its dead cool"
date:   2014-03-12
desc: "second day show to change the background color every second using javascript ,its mind blowing  ...better Mind it...lets start the ride ...Rocknrolla"
---



Second day show....i am the man Rocknrolla song is been playing in my mind.in this good day i will try javascript which changes the background color as per the time ,i.e for each and every second the background color will get changed gradually.

###our plan to execute this:

+ we need to get the time and split it in to hours,minutes and seconds.
+ we know already that colors are basically from RED,BLUE,GREEN.
+ so we manipulate red color from hours,green color from minutes and blue color from seconds.
+ we will be converting the value in to hexadecimal using tostring(16).

    	red = Math.round(255 * (hour / 23)).toString(16);
		green = Math.round(255 * (min / 59)).toString(16);
		blue = Math.round(255 * (sec / 59)).toString(16);

+ we mulitply using 255 is maximum it should be 255 ,as we knew 255,255,255 is white.
+ we pass these values in to formatcolor() function if it is double digit then we will append '0' in the beginning.
+ then combined value of red blue and green is used as body background color. that is accompolished using 

	$('body').css('background-color','#'+color);

+ I need to format the time ,if number is less than 10 then i need to append zer0 in it.
+ using awesome CSS we made it BIG.
{%highlight javascript%}
(function(){

	function time(){

	var time_now = new Date();
	var hour = time_now.getHours();
	var min = time_now.getMinutes();
	var sec = time_now.getSeconds();

	var getTimeColor = function(hour,min,sec){
		
		red = Math.round(255*(hour/23)).toString(16);
		green = Math.round(255*(min/59)).toString(16);
		blue = Math.round(255*(sec/59)).toString(16);
		red = formatColor(red);
		green = formatColor(green);
		blue = formatColor(blue);
		//alert((red+green+blue).toUpperCase());
		return(red+green+blue).toUpperCase();
	}

	function formatTime(j){
		if(j<10){
			j = '0'+j;
		}
		return j;
	}

	function formatColor(k){
		if(k.length < 2){
			k = '0'+k;
		}
		return k;
	}

	var color = getTimeColor(hour,min,sec);
	hour = formatTime(hour);
	min = formatTime(min);
	sec = formatTime(sec);
	
	$('#cur_hour').text(hour);
	$('#cur_min').text(min);
	$('#cur_sec').text(sec);
	$('body').css('background-color','#'+color);
	$('#cur_color').text(color);

	t = setTimeout(function(){
		time();
	},500)
	
}	

time();


}

)();

{%endhighlight javascript%}

####Now its time to append CSS.it is damn simple CSS no need of any explanation

{%highlight css%}
body {
	min-height: 100%;
	position: relative;
	font-family: "Century Gothic", CenturyGothic, AppleGothic, sans-serif;
	background-color: #000;
	color: white;
}

h1{
	font-size: 40px;
	padding:60px 0px;
	text-shadow: -1px -1px 1px rgba(0,0,0,0.6);
	}

#cur_color{
	font-size: 35px;
}

.cur-time{
	display:inline;
	float:center;
	font-size: 35px;
}
{%endhighlight css%}

####And finally HTML below:

{%highlight html%}
<!DOCTYPE html>
<html lang>
<head>
  <meta charset="utf-8">
  <title>click clock | its me</title>
   <link rel="stylesheet" href="reset.css">
  <link rel="stylesheet" href="color-clock.css">
  <script src="http://ajax.googleapis.com/ajax/libs/jquery/1.9.1/jquery.min.js"></script>

 </head>

<body>
	<div id ="wrapper">

		<div id ="container">
			<h1 id="time_color">Current Color is :</h1>
			<div id ="cur_color">

			</div>
			<div></div>
			<h1 id="time_title">Current Time is: </h1>
			<p class="cur-time" id ="cur_hour">
						
			</p>
			<p class="cur-time" id ="colon">:</p>
			<p class="cur-time" id ="cur_min">
						
			</p>
			<p class="cur-time" id ="colon">:</p>
			<p class="cur-time" id ="cur_sec">
						
			</p>
				
		</div>
	</div>

</body>
  <script type="text/javascript" src="color-clock.js"></script>
</html>
{%endhighlight html%}

I m the man..........
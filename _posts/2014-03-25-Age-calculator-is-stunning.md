---
layout: post
title:  "Age calculator is stunning "
date:   2014-03-25 
desc: "I am so sorry for not keeping my javascript oath promis,but for sure i will try to keep my daily coding promise.Today we will play with finding our age,days .minustes old . i feel glad to say that i have learnt toDateString(),getTime(),getDay() and how to go back to future date."
---

I am so sorry for not keeping my javascript oath promis,but for sure i will try to keep my daily coding promise.Today we will play with finding our age,days .minustes old . i feel glad to say that i have learnt toDateString(),getTime(),getDay() and how to go back to future date.

####Explanation:
+ get the bith date and split it in to month/day/year .
+ get the birth time and spplit it in to hour/min .
+ get todays date in 'today' variable.
+ convert today to string using .toDateString() --Tue Mar 25 2014 
+ calculate 'age' by subracting todays year with birth year.
+ get birthdate in date format and store it in 'bday' variables.
+ get birthtime in date format and store it in 'btime' variables.
+ calculate ageInDays by (today-bday)/(24*60*60*1000)
+ calculate ageInMins by (today-btime)/(60*1000)
+ calculate tenKdays that is 10000k days from birthdate (bday.getTime()+24*60*60*1000*10000)
+ append it to body.

{%highlight javascript%}

(function(){
	
	var months=['Jan','Feb','Mar','Apr','May','Jun','Jul','Aug','Sep','Oct','Nov','Dec'];	
	$('form').on('submit',function(e){
	e.preventDefault();
	var birthday = $('#bday').val().split('-');
	//getting birthday and splitting it to months/days/year
	var year = birthday[0];
	var month = (birthday[1])-1;
	var day = birthday[2];

	var today = new Date(); //get todays date
	todayStr=today.toDateString(); //convert in to string like Tue Mar 25 2014 
	var bday = new Date(year,month,day); //get the bday date indate format

	var birthTime=$('#btime').val().split(':'); //get birthday time in to hour and mins
	var hour = birthTime[0]; 
	var min = birthTime[1];

	var btime = new Date(year,month,day,hour,min); //convert birthday and time in to date format
	var age = today.getFullYear()-year; // calculate the age using as year from todays year
	//if birthday in this year is not over then reduce add by 1
	if(today.getMonth()<month || (today.getMonth == month && today.getDay()<day)){ 
		age--;
	}
	var ageInDays=Math.floor((today-bday)/(24*60*60*1000)); //converting age in days
	var ageInMins=Math.floor((today-btime)/(60*1000));
	var tenKDays= new Date(bday.getTime()+ 86400000 * 10000);//adding (24*60*60*1000+10000) to time.

	$('.bdays.date').text("your birthday is on "+months[month]+" "+day);

	if(age==1){
	$('.bdays.age.year').text("you are "+age+" year old"); 
	}else{
	$('.bdays.age.year').text("you are "+age+" years old");
	}

	if(ageInDays==1){
		$('.bdays.age.days').text("you are "+ageInDays+" day old");
	}else{
		$('.bdays.age.days').text("you are "+ageInDays+" days old");
	}
	

	if(ageInDays==1){
		$('.bdays.age.mins').text("you are "+ageInMins+" min old");
	}else{
		$('.bdays.age.mins').text("you are "+ageInMins+" mins old");
	}


	if(tenKDays==1){
		$('.bdays.10k').text("you are 10000k day is "+tenKDays);
	}else{
		$('.bdays.10k').text("you are 10000k was"+tenKDays.toDateString());
	}
	
	

});

})();

{%endhighlight%}

####HTML elements below

{%highlight html%}
<!DOCTYPE html>
<html lang>
<head>
  <meta charset="utf-8">
  <title>Talky talk</title>
  <link rel="stylesheet" href="age-info.css">
  <script src="http://ajax.googleapis.com/ajax/libs/jquery/1.9.1/jquery.min.js"></script>

 </head>

<body>
<div id="container">

	<form>
		<input id='bday' type='date' name='bday' placeholder='mm/dd/yy'>
		<input id='btime' type='time' name='btime' >
		<input class='button' type='submit' value='submit'>
	</form>

	<ul>
		<li class='bdays date'></li>
		<li class='bdays age year'></li>
		<li class='bdays age days'></li>
		<li class='bdays age mins'></li>
	</ul>


	<ul>
		<li class='bdays 10k'></li>
		
	</ul>


</div>	
</body>
  <script type="text/javascript" src="age-info.js"></script>
</html>



{%endhighlight%}

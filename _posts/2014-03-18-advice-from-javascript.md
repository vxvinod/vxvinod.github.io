---
layout: post
title:  "advice from javascript"
date:   2014-03-18
desc: "Big advice from javascript,hear it ,learn it ,folllow it you will grow for sure...its from pure javascript ."
---



Hey Guys you might have been waiting to ask me some head blowing questions like what happened to your daily javascript oath. past 3 days no commits ,no blog post .i am so sorry for not bloggin but i feel happy to say that i have started working on javascript web apps compeltely using javascript with render.js,underscore.js,backbone.js. once i complete it i will blog the complete road map of it.
git hub link is : https://github.com/vxvinod/javascript-oath/tree/master/exercise-5-Web-apps

Today as usual i have brought some thing for you in javascript is . Talky talk is our best friend ,it will ask you what bad habbit you have? and you will be answering and based on it , Talky talk will provide you some advice.

####for example:

me: My bad habbit is eating chocolates
Talky talk: your bad habbit is eating chocolates ? dont spoil your tooth.

me: I drink
Talky talk: you drink ?  fight for good cause not for the above.

me: drinking
Talky talk: you drink ?  lend yo hands to society not to the above.

####Explanation:

+ get the input from user and pass it a function called changePronoun().
+ convert starting 'my' to 'your' and 'i' to 'you'. using regex.
+ return the phrase.
+ display the chaged phrase .
+ display the random advice using Math.rand() method.

{%highlight javascript%}
(function(){
    //alert("inside");
	var advice=[ "you are loser",
				"dont do that please",
				"have fun with  yo family",
				"enjoy a lot and be happy",
				"be gangster of your knowledge not to people",
				"fight for good cause not for the above",
				"lend yo hands to society not to the above"
				]

	$('form').on('submit',function(){
		event.preventDefault();
		var text_value = $('#bad_habit').val();
		//alert(text_value);

		var change = changePronoun(text_value);
		//alert(change);
		//alert(advice[Math.floor(Math.random()*7)]);
		$('#habit-teller').text(change+" ?");
		$('#advice').text(advice[Math.floor(Math.random()*7)]);
		$('#bad_habit').val("");

	});

	function changePronoun(phrase){
		var start_With_I = phrase.substr(0,2).toUpperCase();
		var start_With_My = phrase.substr(0,3).toUpperCase();
		console.log(start_With_My);
		var newphrase = "";
		if(start_With_I == 'I '){
			newphrase = phrase.replace(/^I/gi,"you");
		}else if(start_With_My == "MY "){
			newphrase = phrase.replace(/^my/gi,"your ");
		}else{
			newphrase="You "+phrase;
		}

		return newphrase;
	};

					
})();
{%endhighlight%}


{%highlight html%}
<!DOCTYPE html>
<html lang>
<head>
  <meta charset="utf-8">
  <title>Talky talk</title>
   <link rel="stylesheet" href="reset.css">
  <link rel="stylesheet" href="talkytalk.css">
  <script src="http://ajax.googleapis.com/ajax/libs/jquery/1.9.1/jquery.min.js"></script>

 </head>

<body>

	<h2 id="habit-teller">Tell me your bad habit ?</h2>
	<h2 id="advice">here is my advice</h2>

	<form>
		<input id="bad_habit" type="text" placeholder="My bad habbit is ....."><br><br>
		<input id="submit_btn" type="submit" value="submit">
	</form>

	
</body>
  <script type="text/javascript" src="talkytalk.js"></script>
</html>

{%endhighlight%}


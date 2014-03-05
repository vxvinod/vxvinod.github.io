---
layout: post
title:  "TODAY IM INTERESTED IN AJAX,lets clear it"
date:   2014-03-05
desc: "Ajax is for today lets clear it off and move ahead"
---
####AJAX Introduction:
+ AJAX - Asynchronous Javascript and XML.
Ajax is used to fetch the data from the background from the server and
display it on webpage without loading the page.


+ load() is one of ajax method:

####Syntax:
#####$(selector).load(URL,data,callback);

+ URL - URL wish to load.
+ data - set of query string key/value pair to send along with request.
+ callback - function to be executed after  load() is completed sucessfully.


####callback function has three different parameter:

+ responseTxt = contains the resulting content if call is succeed.
+ statusTxt = contains the status of the call
+ xhr = contains XMLHttpRequest object.

As a Simplestart i have tried to load text content from text file and display it
webpage using load function.
{%highlight html%}
<!DOCTYPE html>
<html>
	<head>
		<meta content="text/html;charset=utf-8" http-equiv="Content-Type">
		<meta content="utf-8" http-equiv="encoding">

	</head>
	<body>
		<div id="div1"><h3>Hey guys this is the start of ajax</h3>
		</div>
		<button>Hit me and know what i am </button>
	
		<script src="http://ajax.googleapis.com/ajax/libs/jquery/1.9.1/jquery.min.js"></script>
		<script>
		(function(){
			console.log("i m inside");
			$("button").on('click',function(){
				
				$("#div1").load("what_i_am.txt",function(resp,status,xhr){
					console.log("resp"+resp+"status:"+status+"xhr object"+xhr);
				});
			});
		})();
		</script>
	</body>
</html>
{%endhighlight%}

####Textfile to be rendered
what_i_am.txt

<h2>I am a heavy hitting rubiest</h2>
<p>Will emerge one day</p>

####OUTPUT:
index.html 

resp<h2>I am a heavy hitting rubiest</h2>
<p>Will emerge one day</p>
status:success
xhr object[object Object]
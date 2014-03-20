
---
layout: post
title:  "animate-box is simple and cool"
date:   2014-03-19
desc: "today because of loads of work ,will play with simple application where there will be couple of square shaped boxes where when it is clicked it will animated(flip,crawl,dangle,stretch,jump)."
---


today because of loads of work ,will play with simple application where there will be couple of square shaped boxes where when it is clicked it will animated(flip,crawl,dangle,stretch,jump).

####Explanation:

+ generate more div elements in html file.
+ add animation css properties to CSS file.
+ get the clicked element reference using 'this' keyword and apply random animation.
+ setTimeout will help to reamin the animation for long time.

github link: https://github.com/vxvinod/javascript-oath/tree/master/exercise-6-animate-box


####Javascript file below

{%highlight javascript%}

(function(){

	var animations=['crawl','dangle','flip','stretch','jump'];
	$('.box').on('click',function(){
		var ths = $(this);
		var anim = animations[Math.floor(Math.random()*5)];
		console.log(anim);
		$(ths).addClass(anim);
		setTimeout(function(){
			$(ths).removeClass(anim);
		},5000);
	});
})();
{%endhighlight%}













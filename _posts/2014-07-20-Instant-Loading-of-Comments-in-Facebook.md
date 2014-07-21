---
layout: post
title:  "Instant Loading of Comments in Facebook"
date:   2014-07-20 
desc: "Have you noticed while playing in facebook with your friends by commenting about a topic especially in group chat instantly the comments will get updated without even a single page load. I felt embarassing oh my god how fast is it man though more than 50 friends of mine is commenting on my post it gets loaded like ISRO chandrayan how come i was thinking may at that time i would have been busy in watching vijayakanth movies"
---

##Instant Loading of Comments in Facebook

Have you noticed while playing in facebook with your friends by commenting about a topic especially in group chat instantly the comments will get updated without even a single page load. I felt embarassing oh my god how fast is it man though more than 50 friends of mine is commenting on my post it gets loaded like ISRO chandrayan how come i was thinking may at that time i would have been busy in watching vijayakanth movies but when i am forced to do this in my project so that when an user updates anything it should get reflected to all clients who are in that page. At that time i was confused and slightly afrais oh my god how am i going to do this but after seeing railscasts polling episode it was quite easy and i got confidence that i can also implement this and i came to the power of javascript at that moment really really so powerful.

lets be crisp about how a comment is been loaded instantly without page load.when i was watching railscasts he was explaining with simple content management system which was inherited from 'polymorphic association topic' thank god i have compelted that just two days ago it was bit easy for me to praactice. here we go how to implement instant loading.

###Steps to Implement :
    + when full dom is loaded, check if any comments is available and if it is then call simpleTimeout function which calls
    the function after particular time. 
    setTimeout(this.request, 5000);
    
    + In that callback function call the ajax request which gets the new comments and updates that in to html.
    
    + in get method url is fetched from data-url take which is assigned to article_comments_url(@article) this tells that 
    fetch the comments for that particular post comment alone.
    
    + and in Comment controller index action will be called and in that render the comments partial and after that again 
    call the same function so that it will go like recursive function. so what happens is for every 5 seconds the comments partial will get updated.
    
    + in order to avoid fully loading the comment field we can make it more effecient by just appending the latest comment
    that can be done using assigning last comment by id and check whether if any comments available more than that id then append it to comments view.
    
###code

{%highlight javascript%}
CommentPoller = {
    
	 poll: function(){
	 		setTimeout(this.request, 5000);
	 },

	 request: function(){
	 	$.get($("#comments").data('url'), {after:$(".comment").last().data('id')});
	 }
};

(function(){

	if ($("#comments").length > 0){
		CommentPoller.poll();
	}
})();


{%endhighlight%}

###index.js.erb -- this will be triggered if any ajax request comes

{%highlight ruby%}
$("#comments").append("<%= j render 'travers' %>")
CommentPoller.poll();
{%endhighlight%}

###Controller 
in controller need to specify that respond to ajax request
{%highlight ruby%}
  respond_to do |format|
      format.js
      format.html
    end
{%endhighlight%}

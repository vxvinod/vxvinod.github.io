---
layout: post
title:  "What I Learnt Today"
date:   2015-10-19 
desc: "Time has come to exercise my brain with Ruby and Javascript. I have taken a oath that lean daily about anything and post a blog post about it. no matter about size of the content event it may be 2 lines. each and every drop collected from today will be an ocean soon. so here it goes today getting started from CoderByte. so daily i am going to solve a ruby puzzle from easy level moving to intermediate and then to complex. hope this will help me to gain more knowledge."
---

####Daily Exercise my brain

Time has come to exercise my brain with Ruby and Javascript. I have taken a oath that lean daily about anything and post a blog post about it. no matter about size of the content event it may be 2 lines. each and every drop collected from today will be an ocean soon. so here it goes today getting started from CoderByte. so daily i am going to solve a ruby puzzle from easy level moving to intermediate and then to complex. hope this will help me to gain more knowledge.

This is my first challenge to start from easy level. hope this journey will go till i reach my destiny.

{%highlight ruby%}
def FirstReverse(str)
  # code goes here
  val = str.split('')
  str=[]
  val.each{|x| str.unshift(x)}
  return str.join 
         
end
   
# keep this function call here 
# to see how to enter arguments in Ruby scroll down   
puts FirstReverse(STDIN.gets) 
{%endhighlight%}

####Unshift can do..

Above problem is simple reverse program without using inbuilt function. from that i have learnt that 'unshift' method will insert value at beginning of array.



---
layout: post
title:  "Dynamic proxy acts bridge for unknown methods"
date:   2014-02-08
desc: "Proxy the word it remember my amazing college days , where my friends asked me to sign proxy for them nut i am glad to know that it is also used in Ruby."
---
Proxy the word it remember my amazing college days , where my friends asked me to sign proxy for them nut i am glad to know that it is also used in Ruby.
ruby also uses proxy .lets be staright forward what is proxy? proxy is one authorized object(general object : person) acting for another object.

####Dynamic Proxy:
Forwarding to another object when message from object doesnt match method name.

When a object calls a method whose name is unknown and there is no method in that name which is present in the class then automatically method_missing() will be called and here the current object is wrapped and forwarded to the method called and that particular method is executed.
 
{% highlight ruby %}
class Capgemini

	def java
		puts "Welcome to Java"
	end

	def ruby
		puts "you are fortunate to study Ruby"
	end

	def php
		puts "welcome to php"
	end

	def test
		puts [1,2,3].size
	end
end


class DynProxy 

	def initialize(object)
		@object=object
	end
	def method_missing(name,*args)
	puts "inside ghost"
		puts @object.send(name,*args)
	end

end

dyn=DynProxy.new([1,2,3])
dyn1=DynProxy.new(Capgemini.new)
dyn.size # 3
dyn.reverse #3 2 1
dyn1.java # Welcome to Java"
{% endhighlight%}

####Explanation:

array is initialized
dyn is object initialized for DynProxy class and when it calls size method it would enter in to method missing,because 'size' method is not 
supported by wrapped object and same in the case of 'reverse' method . since wrapped object doesnt support 'length' method it calls method_missing and over
there object is wrapped and forwarded to method calls(size,legth) because bere @object is array we initialized it has to support 'size' and 'length' method.

instance of capgemini class is passed in to DynProxy class initializer 
when dyn1 calls 'java' method it goes to method_missing since method name of java is missing and over there method is wrapped and 
it is forwarded tp java method in Capgemini class because '@object' has instance of Capgemini class it calls java method in that class.
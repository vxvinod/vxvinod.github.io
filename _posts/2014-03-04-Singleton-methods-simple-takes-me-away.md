---
layout: post
title:  "Singleton methods simple takes me away"
date:   2014-03-04
desc: "Singleton methods concept in ruby is amazing and mindblowing .though it is simpple it does manythings at backend and makes user to easy to use,coool lets begin our trek"
---

Singleton methods
---
Singleton methods are nothing but adding method to a single object.

{% highlight ruby%}
class Myclass

    @str = "hello i am mark"
	def @str.check
		self.upcase ==  self
	end

	def self.end
		puts "hello"
	end
	puts Myclass.singleton_methods #->end
	puts @str.singleton_methods #->check
	puts self.singleton_methods #->end
end

cl = Myclass.new
Myclass.end #=> hello
{% endhighlight%}

####Explanation:


We are addding methods to single method such as adding check methods to
@str object. and adding end method to Myclass class(in Ruby class is an object)

####what self keyword does ?


'self' keyword in ruby gives you access to the current object.
in class context self refers to current class(which is instance of class Class)
inside method self refers to current object of method called.

####ADDING SINGLETON METHOD TO CLASS

{% highlight ruby%}
class MyClass
	def self.hello
		"hello"
	end
end
{% endhighlight%}

+ while adding singleton methods to class use self to refer the current class and define the methods

####ADDING SINGLETON METHOD TO METHOD

{% highlight ruby%}
	def mc.hello
		"hello"
	end
{% endhighlight%}

+ while adding singleton methods to method use object.method to define the methods

####EXAMPLE

{% highlight ruby%}
class Myclass
	attr_accessor :title

	class << self
		def author
			"paul"
		end
	end 

	def set_author
		"#{@title} by #{self.class.author}"  #while calling a self method you should call using self.class
	end
end

mc =  Myclass.new
mc.title = "Metaprogramming"
puts mc.set_author
{% endhighlight%}

####Explanation:

for calling a singleton methods from another method use 'self.class.methodname'
####NOTE:

+ Singleton objects are meant provide an effective way of organizing global state and behavior, such as configuration data, logging support, or other similar needs. 

####TO BE SIMPLE :
{% highlight ruby%}
lets dive in to example

class MyClass
	def size
		24
	end
end

mc,hc=MyClass.new

def hc.size
	50
end

puts mc.size # 23
puts hc.size # 50
{% endhighlight%}

+ here mc and nc are objects of class Myclass
+ adding an object(size) to hc(object) and it behaves differently when it is called  this is called singleton methods



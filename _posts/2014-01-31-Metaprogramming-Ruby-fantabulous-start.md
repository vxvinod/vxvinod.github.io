---
layout: post
title:  "Metaprogramming Ruby fantabulous start"
date:   2014-01-31 
excerpt: "Oh my god!!!!!!! how I missed this book “Metaprogramming Ruby” the name was scarring me oh this may be the advanced version of ruby ,I should not read this until I am good enough in ruby.I am damn sure that i am not still beginner in ruby.come lets dive in."
---

####Metaprogramming Ruby

Oh my god!!!!!!! how I missed this book “Metaprogramming Ruby” the name was scarring me oh this may be the advanced version of ruby ,I should not read this until I am good enough in ruby but the Mr Paulo parretto made me clear by asking one question in introduction that made my mind that I am not still beginner in ruby that question was simple “how do you iterate an array ?” thank god I answered using “each” method and you may be surprised what’s in this to get surprised but yes there is big thing to get surprised and that is “if you answer using for loop array iteration can be done then you are not comfortable with ruby go back and kick the basics ass of ruby and come back ” and on other side “if you would have answered using “each” method array iteration can be done then you are not still beginner and you are the best to start kicking this book” .i hope now you would have concluded why I got surprised and excited.


After a fantabulous introduction of Metaprogramming Ruby, now its time to start the ride towards the wonderful island of Ruby and I started reading the first lesson ”Monday The Object Model” believe I was not having even one percent of feeling that I am reading a technical book. It was like a story revolving between Bob and Bill .where they both were involved in refactoring some library management system and here Bob is new to Ruby and Bill is a Ruby expert and most of the times Bill will be taking lead and teach Bob how to optimize aaahh the good word will be refactoring the code by teaching meta programming and Bill also demonstrates using some basic drawing for describing the hierarchy of Class and its super class as it goes on. come lets have a quick discussion about what I have learnt from the chapter1 “Monday The Object Model”.

1.Monday The Object Model
*Metaprogramming allows us to open the existing class and add method in to it .which will be reusable throughout the application.

Code:
{% highlight ruby %}
class Test
def old_method (array,from,to) #old way to define a separate method to do replace operation
array.each_with_index do |x,y|
array[y] = to if x == from
end
end
end

class Array
def element_replace(from,to) #open the Array class and add element_replace method in to it.
self.each_with_index do |x,y|
self[y]=to if x==from
end
end
end
t=Test.new()
#puts t.old_method([2,1,4,3],2,5)
puts [2,1,3,4].element_replace(2,5)
{% endhighlight %}
here in above example –
*old_method in class Test is ordinary way of defining a method which will replace the elements in the array.
*here every time we need to keep in not where the method is added and instead we can open the existing class Array and insert a method to perform the action.
*there is one disadvantage what if the existing method is replaced by Open class methodology for example what if I open existing method downcase method and customize it then rest of the code using downcase method will execute the over written downcase method which will mess up the whole application this is reckless pathching of class and it is called Monkey Patching. The term sound great na!!!!!!!!!!!

####Note:
* to get the methods starting with pattern
{% highlight ruby %}[].methods.grep /^re/ #=&gt;[:replace,:reject,..]{% endhighlight %}
####What is an Object ?
* object which contains bunch of instance variables and reference to the class to which it is initialized.
Obj=Array.new
Obj is an object which contains collection of bunch of instance variable and reference to Array class.

####What is class?
Class is an object which contains bunch of instance methods and links to superclass.

####What is instance methods?
Instance methods are methods of object lives in object class from point of view of class.

####NOTE:
*from class it is instance method and from object view it is method,hope you didn’t get it right will give you an example.
Array.instance_methods == [1,2,3].methods true
Array.methods == [1,2,3].methods false.
This shows that a method is instance_method from class point of view and it is a method from object point of view.
####Note:
*object instance variable live in object itself
and object methods live in object itself

####Structure of Class
*My_class(defined)ObjectBasicObject-
You can check the structure of class using ancestors method for example
My_class.ancestors # [My_class,Object,kernel,BasicObject]
Now I am damn sure that you might think WTF kernel is doing here, ya so sorry I will explain it now aahhh kernel is an module its not a class ,Ruby handles the module in amazing way that it create anonymous class wchich wraps around the module and add the class to the chain.

Sample code
{% highlight ruby %}
module A
def my_name
“Vinod”
end
end

class B
include A
end

class C &lt; B
end
C.new.my_name ”Vinod”
C.ancestors [C,B,A,Object,kernel,BasicObject]
{% endhighlight %}
####Note:
*all classes inherit from Object
*class inherits from Module and it inherits from Object

####Where Modules and Class should be used?
*modules are used when your code is explicit and where it to be included somewhere(used as namespace).
*class is used when it is to be instantiated and inherited.
*Modules XXX which only exists in container of constants is called Namespace.
Note:
*load(filename) -constants will not be loaded as a result it will pollute the program with its own contants
*load(filename,true) -ruby creates anonymous module as namespace and stores the constants
*load()-to execute code
*require()-to import librarbies

####Execute Method
*There are two process two process to execute a method
* method lookup to find the method
* execute the method for that Ruby needs self.
Method lookup reciever and ancestor chain.
*method look up is Ruby searching method that is called in object class and it traverses through ancestor chain till BasicObject.here ruby follows a behavior “go one step right to receiver class and then go up to ancestors chain”.
*I need to tell about receiver ya receiver is an object that calls an object. For example
my_array.size my_array is a receiver .

####SELF
*everyline of ruby code is executed inside an object it is called current object and it is known as self
*when a method is called then object which calls the method is self
*in class/module outside method the self will be taken by class/module.

####How to call Private Method?
*private method can be called only be implicit reciever and cannot be called from explicit receiver.
*we can call the private method that is inherited from superclass
{% highlight ruby %}
code
class Test
private
def private_method
"this s private method"
end
end
class Sample < Test
	def call_private_test
		private_method
	end
end

s=Sample.new
puts s.call_private_test ”this is private method”
puts s.private_method  private method cannot be accessed error
{% endhighlight %}

TANGLE OF MODULES
Take a case that there are two modules (modules are King,Queen) of same method such as (name, age) and both modules are included in to a class(Lovers) and an object instantiated to the class calls a method called age.which method will get executed? I was stumbled when I read this and I guessed the it will execute the first module included here comes the twist of Ruby I will explain how Ruby fix this.

When first module King is included Ruby wraps in to class and adds it to ancestor chain above the base class (Lovers) assume we are considering a stack and last value is base class and Top value is BasicObject(Lovers,King,…BasicObject) .so when next module Queen gets include again ruby wraps in to class and push it above the base class(Lovers,Queen,King…BaseObject).


{% highlight ruby %}
module King
	def name
		"vinod"
	end
	def age
		23
	end
end

module Queen
	def name 
		"katrina kaif"
	end
	def age
		27
	end
end

class Lovers
	include Queen
	include King
	def blast
		puts name
		puts age
	end
end

class Enemy
	include King
	include Queen
	def blast
		puts name
		puts age
	end
end
lov=Lovers.new
lov.blast
puts Lovers.ancestors #=>[Lovers,King,Queen,Object,Kernel,BasicObject]
puts Enemy.ancestors #=>[Lovers,Queen,King,Object,Kernel,BasicObject]
{% endhighlight %}

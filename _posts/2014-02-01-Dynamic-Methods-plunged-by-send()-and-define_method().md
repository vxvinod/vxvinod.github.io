---
layout: post
title:  "Dynamic Methods plunged by send() and define_method()"
date:   2014-02"-01
desc: "I am happy to write my next post on Dynamic Methods  in metaprogramming on very next day to the last post on Chapter 1 .I will be short and crisp to explain about how methods are dynamically created in Ruby in run time."
---

I am happy to write my next post on Dynamic Methods  in metaprogramming on very next day to the last post on Chapter 1 .I will be short and crisp to explain about how methods are dynamically created in Ruby in run time.

Take a example In an Information Technology service organization has various domains such as Java ,Ruby,Php and in each domain there are couple of projects has been acquired recently. So the organization needs a reliable system  which should provide information about resources and projects in each and every domain.

Here comes Mr Vinod heavy hitting rubiest this is to motivate me dont mind it please ,he plans to create a system using ruby language and he creates  a class called DataSource where it contains information regarding resource count and projects count in every domain.

He creates another class name called Domain where its functionality is to fetch results from datasource and render it in to User Interface.so in order to fetch data in every domain he creates two methods (get_resource,get_projects) in all domain so in total 6 methods are there because as i mentioned there are 3 domains(Php,Ruby,Java). So if i add more domains in datasource i need to add 2 more methods in Domain class to perform this operation.oh shit this is not reliable system and most of the code is kind of repetitive so in order to fix this ruby provides amazing feature that is creating Dynamic Methods in run time.

Lets start the roller coaster ..err errrrrrrrrrrrr  

In order to implement Dynamic Method fearture we need to know about send() and define_method().
####Send()</strong>
*Normally we use dot operator to call a method from object as a reciever but in case of creating method dynamically we need to use send().
*Obj.send(:method_name,*args) – first argument is the method name and arguments are passed as subsequent arguements.
####Advantage:
using send we can wait until last minute to pass the method name in to send function so that particular method will be called at run time while
 code is running is called Dynamic Dispatch
####Note:
*obj.send(:method_name,arg1) --method name can be strings or symbols,symbols will be considered //ruby is sending name of method to object

####Difference Between String and Symbol
*Symbols are immutable ,we can change characters inside the string but we cant do in symbols	
*comparison is faster in symbols than string
*convert sting to symbol use String#to_sym()
*convert symbol to string use String+to_s()
####Note:
*Send() can call any method including private method but it breaks the encapsulation and new method public_send() which respects the receiver privacy.

*filtering methods based on their names in class is called pattern dispatching.
####How to use Define method ?
we can define method dynamically using define_method
when define method is executed it creates method hello at run time it is called Dynamic method

{% highlight ruby %}
class Dynmethod

	define_method :hello do |x,y|
		"hello #{x} and #{y}"
	end
end

dyn=Dynmethod.new
puts dyn.hello("vinod","sabari")
{% endhighlight %}
####Refactoring the System retreiving system</strong>

{% highlight ruby %}
class DataSource  #DataSource where it stores the information about resources and project of every domain

 def get_ruby_resource(location_id)
 	"23"
 end

 def get_ruby_projects(location_id)
 	"Clear trip,Twitter,Github"
 end

 def get_java_resource(location_id)
 	"50"
 end

 def get_java_projects(location_id)
 	"flipkart,google+"
 end

 def get_php_resource(location_id)
 	"43"
 end

 def get_php_projects(location_id)
 	"facebook,orkut"
 end

end

class RefDomain

	def initialize(location_id,data_source)
		@id=location_id
		@ds=data_source
		@ds.methods.grep(/^get_(.*)_projects$/){ RefDomain.define_domain $1 }  #here grep filets the methods to matches the particular pattern and stores in $1
	end

	def self.define_domain(name)
		define_method (name) do   #dynamically method is created
		resource=@ds.send "get_#{name}_resource",@id  
		projects=@ds.send "get_#{name}_projects",@id
		conclusion="#{name.to_s.upcase!}:#{resource} resources and #{projects}"
		return "enough #{conclusion}" if resource.to_i >30
		conclusion
		end
	end

end
d=RefDomain.new(2,DataSource.new)
puts d.ruby # > RUBY:23 resources and Clear trip,Twitter,Github
puts d.java # > enough JAVA:50 resources and flipkart,google+
puts d.php # > enough PHP:43 resources and facebook,orkut

{% endhighlight %}

####Explanation of above code:
*here when initialize method is called grep command filters the method which matches the particular pattern (get_ruby_projects,get_php_projects,get_java_projects) here (java,Ruby,php) is stored in global variable in $1.
*RefDomain.define_domain will be called thrice for the strings (Java,Ruby,Php).
*when define_domain is called define_method() is called and Dynamically method for (Java,Ruby,php) is created.
*when new component is added to DataSource the system will definitley will work fine.
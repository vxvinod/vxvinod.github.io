---
layout: post
title:  "Ghost Methods magic of Metaprogramming"
date:   2014-02-04
desc: "Good Morning !!!!! Today let us discuss how to handle methods which are not defined using method_missing() magic of metaprogramming."
---



Good Morning !!!!! Today let us discuss how to handle methods which are not defined using method_missing() magic of metaprogramming.what happens when you go to a house and enquire about people who is not present  in the house definitely they will say “no person in that name here” same in ruby when you call a method which is not really present ,ruby searches it in base class and searches up to ancestor chain up to object and then enters in to kernel.if ruby does not find the method anywhere then it admit the failure by surrendering in to method_missing() method which is instance method of Kernel .which every object inherits . It will throw you “No Method error”

but we are fortunate to use Ruby which provide metaprogramming features to handle method that really doesnt exist by overriding method_missing() .

####GHOST METHODS 
if you call a methods that really doesnt exist Ruby handles it by method_missing() which handles the method and perfroms the necessaary action these are called ghost methods 


*"if they ask you something and you dont understand do this"*



{% highlight ruby %}

class Hunter 
	def method_missing(method_name,*args) 
 	  puts " name #{method_name} and his weight,height : (#{args.join(',')})" 
 	  puts "block is presend" if block_given? 
	end 
end 

hunt=Hunter.new 
hunt.vinod 82,180 do 
puts "this is block" 
end 
{% endhighlight%}


Here in below example we are refactoring the organization puzzle using method_missing where Ghost method is implemented.
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
		#@ds.methods.grep(/^get_(.*)_projects$/){ RefDomain.define_domain $1 }  #here grep filets the methods to matches the particular pattern and stores in $1
	end

	def method_missing(name)
		super if !@ds.respond_to?("get_#{name}_resource")  # pattern doesnt match then call method_missing() in kernel
		resource=@ds.send "get_#{name}_resource",@id  
		ojects=@ds.send "get_#{name}_projects",@id
		conclusion="#{name.to_s.upcase!}:#{resource} resources and #{projects}"
		return "enough #{conclusion}" if resource.to_i >30
		conclusion
	end
	
end
d=RefDomain.new(2,DataSource.new)
puts d.ruby # > RUBY:23 resources and Clear trip,Twitter,Github
puts d.java # > enough JAVA:50 resources and flipkart,google+
puts d.php # > enough PHP:43 resources and facebook,orkut
puts d.net
{% endhighlight%}
####Explanation:
Here when a  unknown method is called it calls the method_missing() and first it checks whether the method name is present in the Datasource if not then call ‘super’ which calls the method_missing() in kernel which throws no method error otherwise it will execute the corresponding method in Datasource.

#### Object Oriented Programming in Ruby

Object-oriented programming (OOP) is a programming paradigm that uses objects and their interactions to design and program applications

- Allows the programming to block off areas of code that perform certain tasks independently of other areas in the application

- **Encapsulation** — hiding pieces of functionality and making it unavailable to the rest of the code base. In summary and usecase as a form of data protection so that data cannot be altered withouth obvious intention.

- **Abstraction** is simplifying a complex process of a program, an eterprise software solution for example by modeling classess appropriate for it.

- **Inheritance** is used where a class inherits the behavior of another class, as referred to as the superclass.

- **Polymorphism** is when a class inherits the behaviors of another class, but has the ability to not inherit everything and change some of it's inherited behaviors. For example to write a method that does something differently from the inherited method.

  ```ruby
  # Example of Inheritance and polymorphism where we change the instance methods according to the classes that inherits from the superclass.
  class GenericParser
  	def parse
  		raise NotImplementedError 
  	end
  end
  class XMLParser < GenericParser
  	def parse
  		puts "An instance of the XMLParser class received the parse message"
  	end
  end

  class JSONParser < GenericParser
  	def parse
  		puts "An instance of the JsonParser class received the parse message"	
  	end
  end

  puts "Using the XMLParser"
  parser = XMLParser.new
  parser.parse

  puts "Using the JSONParser"
  parser = JSONParser.new
  parser.parse
  ```

  ​

- **Classes** is a blueprint that describes the state and behavior that the objects of the class all share. A class can be use to create many objects. Objects created at runtime from a class are called instances of the particular class. 
  - Organizes code into well-organized areas
  - "Carry around" their class's code


#### Some Keywords To remember

* **Instance** - In summary an object that is created from a class.

* **Attributes** - The types of the data that is defined that makes it a class.

* **Initializer Methods** - In Summary boots up all of the attributes that we defined in our attr_accessors method

  Example

  ```ruby
  class Animal 
    attr_accessor :type, :noise
    # "*" splat operator takes in all of the attributes defined in our attr_accessor method
    def initialize(*args) 
      @type = args[0]
      @noise = args[1]
    end
  end

  # instantiate a new object instance of Animal
  # Since we have a initialize method it is understood that the arguments that we pass in from when we are create a new Animal object will be transposed as the arguments "args" in the intialize method.
  cow = Animal.new("Mammal","Moo")
  cow.type # returns "Mammal"
  cow.noise # returns "Moo"
  ```

  ​

* **Instance Variables** - Variables that can only be access from within a instance of an object or within an instance method. 

* **Instance Methods** - methods that are associated with an instance of a which Class, The instance method is declared.

* **Setter and Getter Methods** - Setter and Getter Methods basically gives us access control to our instance variables inside our class Since we can't actually access them from outside of the class we need to define them inside a instance method.

  ```ruby
  # def noise; end # Can be called an instance method since it has a "=" it is classified as a setter method.

  # def get_noise; end # Can also be called an instance method but since were specifying a instance variable inside it is classified as a Getter Method.

  class Animal 
    def noise=(noise) # Setter Method
      @noise = noise 
    end
    # Can only be accessed from inside the method call
    def get_noise # Getter Method
  	@noise 
    end
  end
  cow = Animal.new
  cow.noise = "Moo" # Setter Method Call
  cow.get_noise # Getter Method Call # Returns Moo
  ```


* **Class Methods** - A Class method is a method that is associated with the containing class, in which only a class is able to access that method. The keyword self is used to precede the method name making it a class Method.

  ```ruby
  class Animal
    # Class method definition
    def self.say_hello
      puts "Hello World"
    end
  end
  # Class method call
  Animal.say_hello
  ```

* **Class Variable** - A class variable (@@) is shared among the class and all of its descendants. A class instance variable (@) is not shared by the class's descendants. Note Class Variables can be accessed by any classes that inherits from the super class where the class variables are declared at.

  ```ruby
  class Foo # Foo Superclass
    # Class Variable
    @@num = 1
    
    # Make a class Method
    def self.getter_method
    	@@num 
    end
    # Class setter method
    def self.setter_method=(value)
      @@num = value
    end
  end

  class Bar < Foo # Bar Subclass inherits from Foo
  end

  puts Foo.setter_method = 3 # returns 3
  Foo.getter_method # returns 3
  ```

* **Blocks** - Blocks of code that can be passed into a method just like normal variables.

  To define a block we can write **(&block)** with the preceding ampersand sign.

  ```ruby
  # where number is to be passed in, while &block is the callback block
  def take_block(number, &block)
    block.call(number)
  end

  number = 42
  take_block(number) do |num|
    puts "Blocks being called in the method #{num}"
  end
  ```


*  **Procs and Lambdas** - Similar to javascript's anonymous function, Where methods or blocks of code functionality that can be store inside a variable.

   ```ruby
      Procs:full_name = Proc.new {|first,last| first + " " + last}
            full_name.call("John","Doe")
   ```



* **Overriding Class Method** -  The purpose of overriding specific values from an old class to an new inherited class

  ```ruby
  class Add 
  	attr_accessor :num
  	def initialize(num1,num2)
  		@num1 = num1
  		@num2 = num2
  	end
  	def self.add_values(num1,num2)
  		addition = Add.new(num1,num2)
  		# call our instance method from the class method
  		addition.do_add_values
  	end
      def do_add_values # do the initial addition here
  		"add num1 to num2 = #{@num1 + @num2}"	
  	end
  end

  class Sub < Add
  	# Override the add function from the SuperClass then do the actual subtraction in our "Sub" Class
  	#Class Method instantiation of a new class object
  	def self.sub_values(num1, num2)
  		subtraction = Sub.new(num1,num2)	
  		subtraction.do_sub_values
  	end
      def do_sub_values # do the initial subtraction here
  		"subtract num1 to num2 = #{@num1 - @num2}"
  	end
  end

  puts new_num = Add.add_values(10,20) # Addition Functionality
  puts new_sub_num = Sub.sub_values(9, 15) # Subtraction Functionality
  ```


* **Super Keyword** - In most cases we don't want to override the super class's methods entirely. But change some functionality and tweak some code for better reusability. super calls the first instance of the parent class, then do the actual functionality from that method.

  ```ruby
  class SuperClass
    def someMethod
      "I'm a super method inside a super class"
    end
  end

  class SubClass < SuperClass
    def someMethod
      super + " SubMethod"
    end
  end

  # Instantiate a new object with our SuperClass
  instanceof_SuperClass = SuperClass.new
  puts instanceof_SuperClass.someMethod

  # Instantiate a new object with our SubClass
  instanceof_SubClass = SubClass.new
  puts instanceof_SubClass.someMethod
  ```



* **Modules** - are wrappers around Ruby Code
  * Modules can't be instantiated
  * Modules are used in conjunction with classes
  * Modules are used for classes to avoid namespace pollution. Can be defined with a Preceding Module Name then in between the class and the module we append (::). to declare it as a class that is wrapped around a module.
  * Modules are usually kep in separate files
  * Modules files can serve as code libraries


**Module Example**

```ruby
# Example
# Wrap Classes inside modules to avoid name space pollution
module Romantic
	class Date
        attr_accersor :date
      
      def initialize(date)
        @date = date
      end
      
      def when_date
        @date
      end
	end
end

# Now if we want to get a new instance of class Date We need to append the module name and in between add in the double colons preceding the class and the class method new
dinner = Romantic::Date.new("Aug, 22, 2016")
```

* **Modules as Mixins** - Mixins are pretty much the ability to eliminate the need for multiple inheritance, providing a facility called a mixin.
  * On another note mixins can help you add more functionality if included in a class and everything we wrote in a module or any class that includes the module can be inherited by the class or any sub classes that inherits to the super class.


**Example 1**

```ruby
module ContactInfo
  # attr_accessor serves as our instance variables.
  attr_accessor :first_name, :last_name, :city , :state, :zip_code

  def full_name
      return @first_name  + " " + @last_name	
  end
  def city_state_zip
      csz = @city
      csz += ", #{@state}" if @state
      csz += ", #{@zip_code}" if @zip_code
  end
end

class Person 
  include ContactInfo	
end

class Student < Person # Inherits from class Person then includes the ContactInfo
  include ContactInfo
end

class Teacher
  include 
end

person = Person.new
person.first_name = "Patrick"
person.last_name = "Sunico"
```

**Example 2 instantiating an object with a class method** 

* First familiarize with the module that wraps around our Date method, 
* Second we declared a class method inside of class Date, then there we assigned our values,
* Third we included our module name RomanticDate so we can declare the methods inside there as instance methods of class Date.
* Fourth store the return value of the new instantiated date object inside our set_date variable.
* Finally, now set_date now can be classified as the collection of instances and values assigned from class Date. Now we can access it's instance methods included in our Romantic Date Module

```ruby
module RomanticDate 
	# We can also skip making a initializers method when we have attr_accessor attributes inside our class or module
	attr_accessor :date, :location
	def when_isdate 
		"Our date is at #{@location}, at #{@date}" 
	end
end

module Wrapper
	class Date
		def self.instantiate_date
			new_date = Date.new
			new_date.date = "Aug 22, 2016"
			new_date.location = "Paris, France"
          	return new_date # returns new_date object to be accessed somewhere else
		end
		include RomanticDate
	end
end

# to access class date we must first be able to access our wrapper module preceding the class Date.
set_date = Wrapper::Date.instantiate_date
set_date.when_isdate
```

**Module Actions**

* **Load** - Same as Require, but doesn't keep track of whether or not that library has loaded..

* **Require** - allows you to load a library and prevents it from being loaded more thant once. (Commonly used)

* **Include** - When including a module into a class, it's as if you took the code defined within the module and inserted it within the class.

* **Extend** - We are adding the **module's defined methods as class methods** instead of instance methods. (Usually used for Class Methods)

  ```ruby
  module Log
    def class_type
      "This class is of type: #{self.class}"
    end
  end
    
  class TestClass
    extend Log #extend all of the methods defined in module log and make them as class methods
  end
    
  test_class = TestClass.class_type
  puts test_class
  ```

* **Modules: Enumerable as a mixin** - provides collection classes with several traversal and searching methods. and with the ability to sort. Basically an Enum is a mixin that contains sevel sort and search methods

  * Commonly use on Arrays, Hashes, Ranges, Strings

**Example 1**

```ruby
class ToDoList
	include Enumerable
	attr_accessor :items
	
	def initialize
		@items = []	
	end
	
	def each
		@items.each { |item| yield item}
	end
end

list = ToDoList.new
list.items = ['laundry','dish','vacuum']
# search for a string that has a total of 4 characters
list.items.select {|i| i.length <= 4}
```





​	

​	














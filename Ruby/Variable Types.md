### Variable Types

#### 1. Local Variables

```ruby
x = 10
```

#### 2. Global Variables

A Global variable that is available for the rest of the code. (Not usually used)

As we can see in the code below the global variable we declared from inside a times scope is being printed out outside of the scope. Which truly unidealistic when it comes to programming.

```ruby
x.times do 
  $x= 10
end
puts $x # <- returns 10 
```

#### 3. Instance Variables

An instance variable is a variable that is available to that instance, Instance variables can also be used as a variable that be interpolated through a view , Kind of like a MVC type framework.

```ruby
@Car = "Toyota"
```

#### 4. Constant

Basically a value that can never change once initialized or set a certain value. However in ruby always remember the last line in a ruby program will always be executed. instead of the first one. The Example below shows How the last line will be printed out instead of the previous initialized CONSTANT_VARIABLE.

```ruby
CONSTANT_VARIABLE = "String"
CONSTANT_VARIABLE = "New String" 

puts CONSTANT_VARIABLE # < - prints out "New String"

```

#### 5. Class Variable

A class variable that is only available to that class Instance. 

```ruby
class myClass
  @@team = [1,3,5,6] #<- use the double "@@" to declare a class variable from inside the scope of this class.
end
```


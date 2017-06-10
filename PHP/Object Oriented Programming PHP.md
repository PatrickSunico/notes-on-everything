## Object Oriented Programming PHP

### PHP Classes

Things to note: 

* -> - Unlike in some programming languages we use the "." notation to access methods in PHP we use the "->" arrow symbol to access the methods inside a class.
* Instantiating a new Object from a class, we Define the Instance of an object; prefixed with a "$" along with the name of the object we want to instantiate.

```php
class Car  {
  var $wheel = 4;
  var $doors = 4;
  var $motor = "General Motors";
  function MoveWheels() {
    echo "Wheels are moving \n";
  }
  function driverOwner() {
    // $this refers to the Class Name Car 
    $this->owner = "Anonymous";
  }
}

// Object Instatiation
$Honda = new Car;
// Object Accessor Method
$Honda -> MoveWheels();

// Accessing Properties 
echo $Honda->wheel . " wheels \n"; 
echo $Honda->doors . " doors \n";
echo $Honda->motor. "\n";

// Assigning Properties without overwriting the existing values inside the class 
$Honda->wheel = 8;
$Honda->make = "Honda Motors";
echo $Honda->wheel . " wheels \n";   
echo $Honda->make . " \n";

// Accessing a function and setting a value outside of the Function's Class
$Honda->owner = "Test \n";
echo $Honda->owner;
```



### Class Inheritance 

```php
// Parent 
class Plane {
  var $wheels = 3,
      $doors = 4,
      $wings = 2;
      
  function move_wheels() {
    echo $this->wheels = "Moving \n";  
  }
  
  function open_doors() {
    echo $this->wheels = "Doors opening \n";
  }
}

if(class_exists("Plane")) {
  echo "class exists \n";
}

$new_plane = new Plane; 
$new_plane -> move_wheels();
$new_plane -> open_doors();

//Child 
// Class Motor inherits values from class Plane 
class Motor extends Plane {
  var $horsepower = "v8 \n";
}

$new_motor = new Motor;
$new_motor->horsepower = "v12 \n";

$new_motor->wheels = "8 ";

echo $new_motor->horsepower . $new_motor->wheels . "wheels";
```

### Class Constructors 

A constructor and a destructor are special functions which are automatically called when an object is created and destroyed. The constructor is the most useful of the two, especially because it allows you to send parameters along when creating a new object, which can then be used to initialize variables on the object. Here's an example of a class with a simple constructor:

```php
// Class Constructors 
class Animal {
  function __construct($sound) {
    echo $this -> sound = $sound ;
  }
}
$Cow = new Animal("Moo \n");
$Snake = new Animal("Hiss \n");
$Chicken = new Animal("Cock-a-doodle-doo \n");
```



### Public, Private, Protected Variables / Data Accessibility 

* public - means the variable is available throught the whole program
* protected - means it's only available to this class or any sub classes by means of inheritance as long as we define it inside a function that it inherits from the Super Class.
* private - can only be accessed by the class it was set to be defined.

```php
// Data Access 
class Car {
  public $wheels = 4; // public var means the variable is available throughout the whole program
  protected $hood = 1; // protected means it's only available to this class or any sub classes by means of inheritance as long as we define it with a function;
  private $engine = 1; // private will only be accesed by this class 
  
  // Private accessor method 
  function showPrivate(){
    echo $this->engine;
  }
}

class Truck extends Car {
    function showProtected() {
    echo $this->hood;
  }
}

// Accessing protected methods/variables
$GMC = new Truck();
echo "Truck Wheels =" . $GMC->wheels . "\n";
echo "Engine ="; $GMC->showProtected();

// Accessing private methods/variables 
$Chevrolet = new Car();
echo $bmw->showPrivate();
```






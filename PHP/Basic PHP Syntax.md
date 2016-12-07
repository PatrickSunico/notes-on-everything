### Basic PHP Syntax



```php
<?php ?> opening and closing tags
  
<?php
  echo("Hello World"); // equivalent to print
?>

<?php
  print ('hello world');

  $name = "Nigga";
  echo "Hello" . $name . "<br>"; // . represents concatenating strings
?>
```



### Variables

```php
    <?php
      // storing variables 
      // Boolean
      $loggedIn = true;
      // Integer
      $myAge = 26;
      // Decimal / Floats
      $totalPrice = 146.28;
      // String
      $fullName = "Brad Hussey";
    ?>
```

### Constants

```php
<?php
  // Constant Variables
  define("Title", "PHP Variables & Constants");
  // echo constant variable name without "" string quotations;
  echo Title;
?>
```

#### define keyword

```php
define('NameOfConstantVariable', 'ValuetoBeShown');
```

### Arrays

#### array keyword 

```php
//Regular 
array(1,2,3,4);

// Associative 
array(
	"key" => "value"
)
```

####  Keys Value Pairs / Associative Arrays

```php
$people = array(
        
            "username" => "johndoe",
            "fullName" => "DohnJoe",
            "age" => 32,
            "gender" => "Male",
            "country" => "Mexico"
        );
        
        echo $people["username"]. "<br>"; // $variable user and it's corresponding key from the array
        echo $people["fullName"]. "<br>";
        echo $people["age"]. "<br>";
        echo $people["gender"]. "<br>";
        echo $people["country"]. "<br>";
```

#### Multidimensional Arrays 

```php
<?php
  // Multidimensional Array
  $employees = array(
    array(
          "username" => "johndoe",
          "fullName" => "DohnJoe",
          "age" => 32,
          "gender" => "Male",
          "country" => "Mexico"
    ),
    array(
          "username" => "janeDoe",
          "fullName" => "DaneJoe",
          "age" => 26,
          "gender" => "Transgender",
          "country" => "Mexico"
    )
  );

  echo $employees[0]["username"];
  echo $employees[0]["fullName"];
  echo $employees[0]["age"];
  echo $employees[0]["gender"];
  echo $employees[0]["country"];


  echo $employees[1]["username"];
  echo $employees[1]["fullName"];
  echo $employees[1]["age"];
  echo $employees[1]["gender"];
  echo $employees[1]["country"];
?>
```

### Loops 

#### While Loop 

```php
//counter variable
$num = 0; 
while($num != 10) { //condition
  echo $num . "\n";
  $num++; // increment
}
```

#### For Loop 

```php
for($i = 0; $i < 10; $i++) {
  echo "soi" . "\n";
}
```

#### ForEach Loop / Array Looping or Object Looping

```php
$numbers = array(1,2,3,4,5,6); // iterated numbers
foreach($numbers as $number) { // numbers array identified as number
  echo $number . "\n";   
}
```

Similarly in Javascript

```javascript
var arr = [1,2,3,4,5];
arr.forEach(function(index){
  console.log(index); // index defined as an identifier
});
```

### Functions 

#### Functions with Parameters

```php
//functions with parameters
function calculate($num1, $num2) {
  $sum = $num1 + $num2;
  echo $sum;
}
calculate(23,34);
```

#### Return Values from functions 

```php
//Example 1
function calculate($num1, $num2) {
  $sum = $num1 + $num2;
  echo $sum;
}
calculate(23,34);

// Example 2 
function add($num1, $num2){
  $sum = $num1 + $num2;
  return $sum;
}

$result = add(200,345); // pass $num to result
echo  "Sum is :" . $result;

function subtract($num3, $addResults) {
 $difference = $num3 - $addResults; 
  return $difference;
}

$result2 = subtract(230,$result);
echo "Difference is :" . $result2;
```

### Built in PHP Functions

#### Math Functions

```php
// Built in PHP Functions
// pow
echo pow(2,3) . "\n"; // 2 * 2 *2 = 8

//rand
echo rand() . "\n"; // random number
echo rand(1,10) . "\n"; // range from 1 - 10

//square root
echo sqrt(100) . "\n"; // square root of 100
```

#### String Functions

```php
//String Functions
// string length
$string = "Testing";
echo strlen($string);

// Practical Use Password Length
$password = "Tuborg93";
$length = strlen($password);

if($length <= 3) {
  echo "Password too short" . "\n";
}
else {
  echo "Login Successfully" . "\n";
}

//Uppercase string
$string = "test";
echo strtoupper($string);

//LowerCase string
$string = "TEST";
echo strtolower($string);
```


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
array(
	"key" => "value"
)
```

####  Keys Value Pairs

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


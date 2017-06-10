### Basic PHP Syntax

```php
<?php ?> opening and closing tags
  
<?php
  echo("Hello World"); // equivalent to print
?>

<?php
  print ('hello world');

  $name = "Test";
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
          "gender" => "Test",
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
  echo "soi" . "\n"; // prints soi 10 times
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

.

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

#### Array Functions 

```php
// Array Functions
// max value in an array
$list = [34,55,23]; 
echo max($list) . "\n"; // 55

//min value 
echo min($list). "\n"; //23

// Sort 
// sorts the array from lowest to highest
sort($list);
print_r($list) . "\n";
```

#### Handling Form Data (Basic)

```php
<?php
  // $_POST gets all form data once sent
  // if submit button clicked echo our successful
    if(isset($_POST['submit'])){
      // echo "Submit Succesful";
      $names = array("test-kun", "Tyrone-desu", "Jamal-sama");
      $username = $_POST['username'];
      $password = $_POST['password'];
      // echo $username . "\n" . $password;
      if(strlen($username)  < 5) {
        echo "<h4>Username too short 7-16 characters required</h4>";
      }
      elseif (strlen($username) >= 10){
        echo "<h4> Username characters too long </h4>";
      }
      // in_array checks if a string or name matches somewhere in an array.
      // checks if username is matching inside the names array
      if(in_array($username, $names)) {
        echo "Welcome Back, " . $username;
      } else {
        echo "Invalid Username";
      }
    }
?>
```

#### PHP $_POST Super Global validation

```php
<?php 
  	// on button submit check the values from the form
	if(isset($_POST['submit'])) {
  		$username = $_POST['username'];
      	$password = $_POST['password'];
      	
      	// if username and password exists on form submit 
      	if($username && $password) {
  			echo "Validated Successfully";
		} else {
  			echo "Please enter a Username or Password";
		}
	}
?>
```

#### PHP MYSQL Connection

```php
<?php 
  	// mysqli_connect('which_server','mysql_username','mysql_password','database_name')
	$connection = mysqli_connect('localhost', 'root', 'root', 'loginapp');
	// check if connection is success or failure 
	if(!connection) { 
      die("Database Connection Failed");
    }
?>
```

#### PHP PDO MYSQL Connection

```php
<?php
  $server = 'localhost';
  $username = 'root';
  $password = 'root';
  $database = 'auth'; // database name

  try {
    // PDO method most secure way in doing database transactions
    $conn = new PDO("mysql:host=$server;dbname=$database", $username, $password);
  } catch (PDOExceptiom $e){ // if error on connection
    die("Connection Failed" . $e->getMessage());
  }
?>
```



### PHP CRUD

#### Global keyword in functions

In PHP global variables must be declared global inside a function if they are going to be used in that function.

```php
// global variables
$a = 1;
$b = 2;
function Sum() {
  // global variable defined inside the Sum Function
  global $a,$b;
  
  $sum = $a + $b;
  return $sum;
}

$result = Sum();
echo $result;
```

The above script will output *3*. By declaring $a and $b global within the function, all references to either variable will refer to the global version. There is no limit to the number of global variables that can be manipulated by a function

#### PHP Inserting Data into mysql Table (CREATE)

**Standalone**

```php
<?php
  //make sure your database is connected properly into your php application
  $connection = mysqli_connect('localhost', 'root', 'root', 'database_name');
  if($connection) {
  	echo "Connected";
  } else {
  	die("Database Connection Failed");
  }

  // make a standard insert query command
  $query = "INSERT INTO users(username, password)";
  // Concatenate the values from the form to the query
  $query .= "VALUES('$username', '$password')";
  $result = mysqli_query($connection, $query);
  
  // if the result is false or there is an error
  if(!$result) {
   die('Query Failed' . mysqli_error());
  }
?>
```

**As a function**

```php
<?php
  include "db.php"
function createData() {
    global $connection;
    if (isset($_POST['submit'])) {

      $username = $_POST['username'];
      $password = $_POST['password'];
      // sql query Insert data from form fields to users table via username column and password column
      // Create
      $query = "INSERT INTO users(username, password)";
      // Concatenate
      $query .= "VALUES('$username', '$password')";
      $result = mysqli_query($connection, $query);

      // if $result is false
      if(!$result) {
        die('Query Failed' . mysqli_error($connection));
      } else {
        echo "Record Created";
      }
    }
  }
?>
```



#### PHP (READ)

**mysqli_fetch_row** = The mysqli_fetch_row() function fetches one row from a result-set and returns it as an enumerated array.

**mysqli_fetch_assoc** = The mysqli_fetch_assoc() function fetches a result row as an associative array.



```php
// SQL Query selects all data from the table
$query = "SELECT * FROM users";
$result = mysqli_query($connection, $query);

if(!result) {
  die('Query Failed' . mysqli_error($connection));
}
```

**As a function**

```php
<?php
  include "db.php"
function readData() {
    global $connection;
    $query = "SELECT * FROM users";
    $result = mysqli_query($connection, $query);
    if(!result) {
      die('Query Failed' . mysqli_error($connection));
    }

    while($row = mysqli_fetch_assoc($result)) {
       print_r($row);
    }
 }
?>
```

#### PHP (UPDATE)

First Create a form that will enable a user to select a row id

```php
<div class="form-group">
  <select name="id">
    <?php
      // returns as row id
      showAllData();
    ?>
  </select>
</div>
```

Then create the function that will select the row and evaluate the data to be updated

```php
// read from db for update
function showAllData() {
  
  // explicitly define global to the $connection variable from the db.php file so the function can access it's data
  global $connection;
  // gather all data from users table
  $query = "SELECT * FROM users";
  $result = mysqli_query($connection, $query);

  if(!$result) {
    die('Query Failed' . mysqli_error($connection));
  }

  // option selector for update
  while($row = mysqli_fetch_assoc($result)) {
    $id = $row['id'];
    echo "<option value='$id'> $id </option>";
  }
}
```

Code for Updating the table

```php
<?php 
  function updateTable() {
  	global $connection;
    if(isset($_POST['submit'])) {
      $username = $_POST['username'];
      $password = $_POST['password'];
      $id = $_POST['id'];
	  // on post set the updated data from where id was selected to be updated
      $query = "UPDATE users SET username = '$username', password = '$password' WHERE id = $id";
      $result = mysqli_query($connection, $query);

      if(!$result) {
        die("Query Failed" . mysqli_error($connection));
      } else {
        echo "Update Successful";
      }
    }
  
  }
?>
```

#### PHP (DELETE)

```php
// Delete from table
function deleteRows() {
  global $connection;

  if(isset($_POST['submit'])) {
    $username = $_POST['username'];
    $password = $_POST['password'];
    $id = $_POST['id'];
	
    // selects which row to be deleted
    $query = "DELETE FROM users WHERE id = $id ";

    $result = mysqli_query($connection, $query);

    if(!$result) {
      die("Query Failed" . mysqli_error($connection));
    } else {
      echo "Record Deleted";
    }
  }
  
}
```
#### Password Hashing with PHP

##### BLOWFISH ENCRYPTION (Standard)

```php
<?php
  $password = "somestringasapassword";
  $hash_Format = "$2y$10$";
  $salt = "iusesomecrazystrings22";
  $hash_and_salt = $hash_Format . $salt;
  $encrypted_password = crypt($password, $hash_and_salt);

  // encrypted password
  echo $encrypted_password; 
?>
```

##### password_hash php function with PASSWORD_BCRYPT

**PASSWORD_BCRYPT** will always have 60 characters in length 

```php
// Standard PASSWORD_BCRYPT
<?php 
	$set_password = "somestring";
	$encrypted_password = password_hash($set_password, PASSWORD_BCRYPT);
	echo $encrypted_password;
?>
  
// PASSWORD_BCRYPT with additional hash options
$options = [
    'cost' => 12,
];
$set_password = "somestring";
$encrypted_password = password_hash($set_password, PASSWORD_BCRYPT, $options);
echo $encrypted_password;
?>
```

#### bindParam() function 

Binds a PHP variable to a corresponding named or question mark placeholder in the SQL statement that was used to prepare the statement. 



#### Difference Between Require and Include

* **include( )** will attempt to load the specified file, but will allow the script to continue if not successfully loaded.


* **require( )** on the other hand will cause a "Fatal Error" to occur if the specified file is not successfully loaded.


### Array Sorting Methods 

```php
function data() {
  $food = array(
    'Fish' => array(
      'data1' => 'Salmon',
      'data2' => 'Pike',
      'data3' => 'Bass'
    ),
      'Fish2' => array(
      'data1' => 'Catfish',
      'data2' => 'Pike',
      'data3' => 'Bass'
    )
  );
  // $objList = json_decode(json_encode($food), false);
  
  foreach($food as $key => $value){
    if(is_array($value)) {
      // echo $value['data1'];
      // echo $key;
      if($key === 'Fish'){
        echo $value['data1'];
      }
      elseif($key === 'Fish2'){
        echo $value['data1'];
      }
    }
  }
  data();
```


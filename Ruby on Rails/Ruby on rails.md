#### MVC Framework in Rails

1. **Model (ActiveRecord)** - Maintains the relationship between the objects and the database, and handles validation, association, transactions and more.
2. **View (ActionView)** - The presentation layer that will be rendered to the browser, controlled by a controller based on the code or decision's made inside of the controller to present the data.
3. **Controller(ActionController)** - The facility within the application that directs traffic, communicates and does certain actions with our database such as, querying, searching, sorting, messaging, as well as the recieving end from user input such as form inputs uploads and etc.

#### What is ActiveRecord?

* "Active Record" -  is a design pattern for relational databases.
  * Allows us to retrieve and manipulate data as objects, not as static rows.


* "ActiveRecord" - Rails implementation of active record pattern.

Example:

```ruby
user = User.new
user.first_name = "Patrick"
user.save # SQL INSERT

user.last_name = "Sunico"
user.save # SQL UPDATE

user.delete #SQL DELETE
```



#### What is ActiveRelation

* Object-oriented interpolation of relational algebra.
* Allows us to do small queries which are chainable (like most Ruby objects)

Example: 

```ruby
users = User.where(:first_name => "Kevin")
users = users.order("last_name ASC").limit(5)
users = users.include(:articles_authored)
```

In SQL Syntax: 

```sql
SELECT users.*, articles.*
FROM users
LEFT JOIN articles ON (users.id = articles.author_id)
WHERE users.first_name = 'Kevin'
ORDER BY last_name ASC LIMIT 5;
```

#### Ruby on rails Terminal Commands 

(Option 1) creating a new starter rails template with sqlite as a default database (without the "")

```
rails new "appname"
```

(Option 2) Creating a new rails template with mysql as a database.

```shell
rails new "appname" -d mysql
```

**Note: on using mysql as a database** - Whenever we use mysql we must first configure the database.yml file inside our rails project folder, and comment out the default database name(usually the name of the project) so mysql won't create the database automatically, also we need to specify the database password which is the password that will access in on our mysql db.

To start your server

```
rails server
```

To see a list of all of our routes defined in a rails application

```
rake routes
```

### 1. Rails Generators

**1.1 Scaffolding Adding all CRUD Actions + Table and Model**

Where Article is the model and the title:string is the schema 

```shell
rails generate scaffold Article title:string description:text
```

OR 

**Without Scaffolding only Model**

```unix
rails generate model "ModelName"
```

Both of the code above will generate a file in our db/migrate folder, with a migration file. which evidently will be transposed as our db model file with rake db:migrate

**Generating a model In Summary:**

Creates file in db/migrate

* File name: 20130100000_create_subjects.rb
* Class name: CreateSubjects
* Inherits from: ActiveRecord::Migration
* create_table :subjects, drop_table :subjects

Creates file in app/models

* File name: subject.rb
* Class name: Subject
* Inherits from: ActiveRecord::Base

**1.2 Generate Controllers with Views**

To generate a controller with a view we can specify it by writing this code in the terminal inside our project folder.

```shell 
rails generate controller "controllername" "viewname"  
```

### 2. Database Migrations

* Set of database instructions
* "Migrate" your database from one state to another.

**2.1 To Generate a migration a migration file**

```shell
rails generate migration create_articles
```

**2.2 To rollback a migration (undo the last migration):** 

```shell
rake db:rollback
```

OR redo the last migration with a specifying migration id

```
rake db:redo VERSION=20130812204447
```

OR rollback to the first version of the migration file

```unix
rake db:migrate VERSION=0
```

**2.3 To run the migration file and create the articles table:**

```shell
rake db:migrate
```

OR if you want to specify which rails environment you want to make the migration file to:

Rails run migration files into our development environment by default.

```unix
# run migration files to production
rake db:migrate RAILS_ENV=production

# run migration files to development
rake db:migrate RAILS_ENV=development(default)

# run migration files to our test enviroment
rake db:migrate RAILS_ENV=test
```

OR if there is an error 

```shell
bundle exec rake db:migrate
```

OR if you want to run only one migration file we can specify it's migration id with this following code. This will only run one migration file inside our migrate folder.

```unix
rake db:migrate VERSION=20130812204447
```

**2.4 Check the current status of migration files**

This determines which is currently updated in our migration files

```unix
rake db:migrate:status
```

##### 2.5 To remove orphaned migration files 

This will rollback the database, then do the migrations again, while removing the orphaned migration files pertaining "****NO FILE****"

```unix
rake db:migrate:reset
```

### 3. Migration Methods

#### 3.1 Table Migration Methods

* create_table (table_name, options) - creates a table and assigns a type and actions to be done in a table.
* drop_table(table_name) - deletes a table
* rename_table(table_name, new_table_name) - renames a current existing table name.

#### Creating a Table format

##### Long hand form

Where t.column is defined as creating a column, "name" as the name of the column, ":type" to specify if it's a string, bool or etc. "options" specify actions to be done in a table.

##### Short hand form

Where "t.type" specifies the column type, "name" as the name of the column, "options" for the actiosn done in a table.

```ruby
# where t is table
create_table "table" do |t|
  # Creating columns in a table

  #long hand, 
  t.column "name", :type options

  #short hand
  t.type "name", options
end
```

Example: 

```ruby
# where users is the table name
# "users" || :users string or symbols
create_table "users" || :users do |t|	
  # longhand form
  t.column "first_name", :string, :limit => 25

  # shorthand form
  t.string "last_name", :limit => 50
  # if user made account it's email will be the default value for that particular user
  # email should not be null
  t.string "email", :default => "", :null => false
  t.string "password", :limit => 40
  
end
```

##### Table Column Types

- binary
- boolean
- date
- datetime
- decimal
- float
- integer
- string
- text
- time

##### Table Column Options

- :limit => size - determines the limit or the size of the field.
- :default => value - whether it should have a default value or not
- :null => true/false - whether the column is allowed to be null or not

For Decimal Columns

We can specify the precision and scale

- :precision  => number 
- :scale => number

#### 3.2 Column migration methods

* add_column(table_name,column_name,type,options) - adds in a column to a table 
* remove_column(table_name,column_name) - removes a column in a table
* rename_column(table_name,column_name, new_name) - renames a column in a table
* change_column (table_name, column_name, type, options) - changes specific attributes for a column name, such as we can change the column_name, the type and it's options

Example:

```ruby
# Alter columns in Users Table

# Add a column in admin_users table
add_column("admin_users", "username", :string, :limit => 25, :after => "email")

#change the column options or type
change_column("admin_users","email", :string, :limit => 100)

# rename an exisiting column inside admin_users table
rename_column('admin_users','password', 'hashed_password')

# search for username in admin_users table
add_index("admin_users", "username")
```

#### 3.4 Index migration methods

* add_index(table,column, options) - used frequently in searching and querying columns in a specific row in a column
* remove_index(table, column) - 

##### 3.4.1  Index migration method options

* :unique => true/false
* :name => "custom_name"

#### 3.5 Execute migration method

Executes any SQL string. given as an argument.

```unix
execute("any SQL String")
```



**Useful tips in Database Migrations**

It is common practice to make new migrations files if we make any changes into our existing migration file like so:

```ruby
# Example 1
#32334242423423424_create_articles.rb file

class CreateArticles < ActiveRecord::Migration
  def change
    t.string :title
    #add a new attribute name description with type text
  end
end
```

This will add a new migration file where we can edit or add new columns in our exisiting table

```shell
rails generate migration add_description_to_articles
```

Then we can add a new column name within the the newly created migration file like so.

```ruby
class AddDescriptionToArticles < ActiveRecord::Migration
  def change
    # First give it the table name meaning the articles table
    # Second give it the attribute name "description" that we want to add as part of the Migration
    # Third give it the attribute type of description which is text
    add_column :articles, :description, :text
    
    #add a new timestamp with a type of datetime
    add_column :articles, :created_at, :datetime

    #add a new updated at timestamp with a type of datetime
    add_column :articles, :updated_at, :datetime
  end
end

```

Then after run this code below to implement the changes in our schema file.

```ruby
rake db:migrate
```

Connect to the database and Export a schema from our database

This will generate a schema file in our db folder.

```unix
rake db:schema:dump
```

### 4. CRUD Operations inside the rails console

To access the rails console we can type: 

```ruby
# default environment development
rails console 

# to specify which development environment
rails console production
rails console test
```

**4.1 Creating Records**

* New/save
  * Instantiate the object
  * set the values 
  * then Save
* Create
  * Instantiate object, set values , save

**Single Method**

To create a new object instance with attributes title and description we can do:

```shell
article = Article.new(title: "This is a test title", description: "This is a test description")
```

To save the newly created instance object into our query we can do:

```shell
article.save
```

**Mass Assignment method**

To create a new object instance without typing article.save we can use the mass assignment method is which more preferred than the single method. This way this gets saved straight through our query database. 

**String Method**

```shell
article = Article.create("title": "This is a test title", "description": "This is a test description")
```

**Hash Method**

Give a hash as an argument depending on the parameters of our table.

```ruby
subject = Subject.create(:name => "First Subject", :position => 1)
```

**4.2 Update**

* Find/save Method
  * Find Record
  * Set Values
  * Save

To edit an existing article:

```shell
# find a subject which has a subject_id of 1
subject = Subject.find(1)
# Returns #<Subject id: 1, name: "First Subject", position: 1, visible: true, created_at: "2016-07-04 04:27:45", updated_at: "2016-07-04 04:34:43">

# update some fields
subject.name = "Initial Subject"
# saves to the db
subject.save
```

Using mass Assignment

```ruby
subject = Subject.find(2)
# Returns #<Subject id: 2, name: "Next Subject", position: 2, visible: true, created_at: "2016-07-04 04:38:15", updated_at: "2016-07-04 04:39:52">

# update some fields using mass assignment
subject.update_attributes(:name => "Next Subject", :visible => true)
```

**4.3 Delete Records** 

* Find/destroy method
  * Find record
  * Destory

To delete an existing article:

```shell
article = Article.find(5) # where 5 is the id
article.destroy # destroy the selected article
```

**4.4 Finding Records**

Finding Records

* Primary key finder
  * Subject.find(2)
  * Returns an object or an error
* Dynamic finders
  * Subject.find_by_id(2)
  * Find all method - Subject.all
  * First/Last methods - Subject.first, Subject.last

**Primary Key Finder**

```ruby
# To find an article by it's id
article = Article.find(2)
```

**Dynamic finders**

```ruby
# To find an article by it's given attributes
subject = Subject.find_by_name("Patrick") # => Will return an object that contains the name

# To query objects based on it's order
Subject.first # => returns the row which is first in the table

# To show all data 
ap Subject.all # in awesome_print format
```

### 5. Query Methods

* where(conditions)

  * Subject.where(:visible => true)
  * Difference between where and find, where "Where" will return an ActiveRelation Object, which can be chained, While find will just return back the object where the id specific id is at.
  * Subject.where(:visible => true).order("position ASC")
  * Does not execute a database call imediately


Example: 

```ruby
# Queries a subject by it's subject id then displays it as a string not an array
subject = Subject.where(:id => 1).first

# where method can be chained
subject = Subject.where(:visible => true).where(:position => 2)
```

* order(sql_fragment) - specifies the sort order of the returned records either, alphabetically, reversed order, by sequential order.

Order SQL Format

```ruby
# where ASC is for ascending , DESC is for descending order
"table_name.column_name ASC/DESC"
# This will order country in ascending order by it's position id
# inside the order method must be encapsulated with double quotes
# Where "position" is the :position attribute while ASC corresponds to being in ascending format.
# As String
country = Country.where(:country_name => "New Mexico").order("position ASC")
# OR 
# As Symbols
country = Country.where(:country_name => "India").order(created_at: :desc)
```

* limit(integer) - limits the results to be or shown or returned.

```ruby
# orders the country by date created then limits it to 1 only
country = Country.order(created_at: :desc).limit(1)
```

* offset(integer) - let's us skip over records before selecting the results to be returned or shown.


```ruby
# Assume we have 20 users that lived in india, skip over the 10 then start showing only the users which are indexed at 11 - 20
country = Country.where(:country_name => "India").offset(1)
```

##### 5.1 Condition expression types

* String
  * RAW SQL as type in a OOP format 
  * **"name = 'First Subject' AND visible = true"** 
  * use carefully when it comes from SQL Injection
* Array 
  * Flexible, escaped SQL safe from SQL Injection 
  * **["name = ? AND visible = true", "First Subject"]**
* Hash
  * Simple, escaped SQL, safe from SQL Injection 
  * **{:name => "First Subject", :visible => true}**
  * Only supports equality, range and subset checking


### 6. Named Scopes

* Queries defined in a model
* Defined using ActiveRelation Query methods
* Can be called like ActiveRelation Methods
* Can Accept parameters
* Requires lambda syntax
* Usually lambdas are evaluated when called not when defined.

Basic Syntax: 

Note that a lambda is a anonymous function in ruby. 

```ruby
scope :active, lambda {where(:active => true)}
```

Or using the stabby symbol to define it is a lamdba,

```ruby
scope :active, -> {where(:active => true)}
```

Similary to when, we can make a Class Method

```ruby
# where scope refers to the ActiveRelation Class that inherits from it, in a class method format self also refers to the ActiveRelation Class.
def self.active
  where(:active => true)
end
```

**Another Example:** 

Lamdba short hand:

```ruby
scope :with_content_type, lambda {|ctype| where(:content_type => ctype)}
```

Class method:

```ruby
def self.with_content_type(ctype)
  where(:content_type => ctype)
end
```

**Examples:** 

```ruby
  # Custom Class Query Methods

  # Checks if records containing whether visible is true/false
  scope :visible, lambda{ where(:visible => true)}
  scope :invisible, lambda {where(:visible => false)}

  # Sorts records in ascending order depending on it's position attributes
  scope :sorted, lambda {order("subjects.position ASC")}

  # sorts records in descending order depending on the date created.
  scope :newest_first, lambda{order("subjects.created_at DESC")}

  #searches through the records for a name resemblance input
  scope :search, lambda {|query|
    where(["name Like ?", "%#{query}"])
  }
```

### 7. Associations 

In Rails, an association is a connection between two Active Record Models.

##### Relational database associations 

* One-to-one
  * Abbreviated as 1:1
  * A classroom "has one" teacher
  * A teacher "belongs to" a classroom
  * A foreign key goes on the teacher's classroom.


* One-to-Many
  * Abbreviated as 1:m
  * A teacher "has many" courses
  * A course "belongs to" a teacher 
  * Foreign keys goes on course table.


* Many-to-Many
  * Abbreviated as m:n
  * A course "has many" students and also "belongs to" many students
  * A student "has many" courses and also "belongs to" many courses
  * Two foreign keys, so they go in a join table.

##### 7.1 One-to-one Associations

Unique items a person or thing can have only of.  Usually used to break up a single table

**Remeber, Any class that has a "belongs_to" should have the foreign key, and is usually called the child table when it comes to one-to-one associations.**

* One-to-one Associations Examples 

  * Classroom has_one :teacher
    * Teacher belongs_to :classroom
  * Employee has_one :office
    * Office belongs_to: Employee

  - Student has _one :id_card
    - IdCard belongs_to :Student
  - Customer has_one :billing_address
    - BillingAddress belongs_to :Customer 
  - Subject has_one :page
    - Page belongs_to :Subject

##### has_one Methods

```ruby
# Subject Table
class Subject < ActiveRecord::Base
  has_one: Page
end

# Page Table 
class Page < ActiveRecord::Base
  belongs_to :subject
end
```

##### In rails Console

```ruby
#Search for an existing Subject
subject = Subject.find(1) # returns a subject with a primary key of 1

# prebuilt rails method looks through pages table that has a subject_id = 1, Since we defined it to have a foreign key that can be matched with our subjects table primary key

# Since we defined Subject has_one :page, they are now associated with each other
# Subject has_one :page
subject.page # returns an SQL statement
# SELECT `pages`.* from `pages` WHERE `pages`.`subject_id` = 1

# create a new instance for our Pages Table, assign it's values
first_page = Page.new(:name => "First Page", :permalink => "first", :position => 1)

# Since first_page is an instance of Page, and Page belongs to the Subject Model they are now associated with each other

# This is where we do the actual Association
# will insert the instance of Page, And it's values to our pages table including the subject_id automatically.
subject.page = first_page # will return an SQL Statment 

# INSERT INTO `pages` (`name`, `permalink`, `position`, `subject_id`, `created_at`, `updated_at`) VALUES ('First Page', 'first', 1, 1,
 '2016-07-05 09:35:33', '2016-07-05 09:35:33')

# After Assigning it's values in a SQL format it then queries for the subject_id that  contains 1, Then displays it 

# SELECT `pages`.* from `pages` WHERE `pages`.`subject_id` = 1

# Where first_page is an instance of Page.
# Page belongs_to :subject
first_page.subject

# now it will save the Subject's primary key, into our Pages table as a foreign key to map it's relationship together.
```

To Unassociate them We can either do a destroy method or unassign the subject_id value from the pages table.

```ruby
# destroy method
first_page = Page.find(1)
first_page.destroy

# Unassigning the foreign key from the table
first_page = Page.find(1)
first_page.subject_id = nil
first_page.save
```

##### 7.2 One-to-Many Associations

* Differences from one-to-one associations
  * More Commonly used
  * Plural relationship names
  * Returns an array of objects instead of a single object
* Used when an object has many objects which belong to it exclusively


* One-to-many Associations Examples:
  * Teacher - Course
    * Teacher has_many :courses
    * Course belongs_to :teacher
  * Photographer - Photographs
    * Photographer has_many :photographs
    * Photograph belongs_to :photographer
  * Subject - Page
    * Subject has_many :pages
    * Page belongs_to :subject

##### has_many Methods

```ruby
subject = Subject.find(1)
subject.pages # will return an array

# Using the append operator allows us to add one, from the existing the set that is already there. Without having to redefine them all.
subject.pages << page # appends a page on a set of pages

# Using the array method, But then we would have to specify every page that we want to be inserted in our pages table.
subject.pages = [page, page,page]

# to remove a page 
subject.pages.delete(page) 
# Or
subject.pages.destroy(page)

# to remove all of the pages 
subject.pages.clear

# to check and see if it's empty
subject.pages.empty?

# to find out the size of the array.
subject.pages.size
```

Example:

```ruby
# First Define the attributes of the instance of parent class Subject
subject_1 = Subject.create(:name => "Example Subject", :position => 1, :visible => true)

# where subject_1 = subject_id = 1, Primary key 
# Now define your sub class's instance attributes.
# We need to specify an instance first before saving it therefore we use the .new class method.
example_page = Page.new(:name => "Example Page", :permalink => "first", :position => 1)

# Then we can assign the instance of Page to the subject.pages instance method. Automagically assigns the foreign key of example_page when inserted into the pages table.
subject.pages << example_page

# After inserting the page into the pages table it returns also an SQL statement

# INSERT INTO `pages` (`name`, `permalink`, `position`, `subject_id`, `created_at`, `updated_at`) VALUES ('First Page', 'first', 1, 1,'2016-07-05 11:37:14', '2016-07-05 11:37:14')

# Then Queries it back with another SQL statement
# SELECT `pages`.* FROM `pages` WHERE `pages`.`subject_id` = 1

# example_page is now associated with the first row of the subjects table

# Create another one
subject_2 = Subject.create(:name => "Second Subject", :position => 2, :visible => true)
page_2 = Page.new(:name => "Second Page", :permalink => "second", :position => 2)

# page_2 is now associated with the subjects table. via it's foreign key
subject.pages << page_2
```

##### 7.3 Many-to-many Associations: Simple

* Used when an object has many objects which belong to it but not exclusively


* Many-to-many Examples:
  * Course has_and_belongs_to_many :students
    * Student has_and_belongs_to_many :courses
  * Project has_and_belongs_to_many :collaborators
    * Collaborator has_and_belongs_to_many :projects
  * AdminUser has_and_belongs_to_many :pages
    * Page has_and_belongs_to_many :admin_users
* Compared to one-to-many associations
  * Requires a join table
  * Two foreign keys; index both keys together
  * No primary key column (:id => false)
* Same instance methods get added to the class

##### 7.3.1 Join Table Naming

When doing a simple join. We want only the foreign keys from where the tables are referencing from.

Example: 

* first_table + _ + second_table
  * Both table names are plural
  * Alphabetical order
  * Default name, can be configured
* Example in Alphabetical Order:
  * Project - Collaborator
    * collaborators_projects
  * BlogPost - Category
    * blog_posts_categories

Some Basic Implementation

Create the migration file for the table we want to specify. In the example below we are creating the join table where we assign the foreign keys of the AdminUsers table and the Pages table

```ruby
# from the Migration file
class CreateAdminUsersPagesJoin < ActiveRecord::Migration
  def up
    # Override the default primary key that rails give out for this table
    create_table :admin_users_pages, :id => false do |t|
      t.integer "admin_user_id"
      t.integer "page_id"
    end
    # to specify which columns we want to index altogether including the table name which is :admin_users_pages
    add_index  :admin_users_pages, ["admin_user_id", "page_id"]
  end
  def down
    drop_table :admin_users_pages
  end
end
```

Associating the two Models 

Admin User has and belongs to many pages, Likewise with the Page model

```ruby
# AdminUser Model
class AdminUser < ActiveRecord::Base

  # To configure a different table name
  #self.table_name = "admin_users"
  has_and_belongs_to_many :pages
end
```

Page Model has and belongs to editors which is a naming convention which has a class of AdminUser.

```ruby
#Page Model
class Page < ActiveRecord::Base
  has_and_belongs_to_many :admin_users
  
  # Or we can specify it with another name that we can give instead of the rails default naming conventions.
  has_and_belongs_to_many :join_table => "admin_users_pages"
  
  # without the appending class_name rails will assume that there is a model name called "Editor", but with this we are changing the name of it's association and appending in which class it belongs to.
  has_and_belongs_to_many :editors, :class_name => "AdminUser"
end
```

##### In rails Console

```ruby
# Many-to-many simple Association
# Assign a new admin to a page
# Find a page to be assigned
page = Page.find(1)

# Next create a new admin and assign the attributes
new_admin = AdminUser.create(:first_name => "Patrick" :last_name => "Sunico", :email => "PS@gmail.com", :username => "PS")

# then we can append the new admin we created to a page and join their primary keys as foreign keys together in another table.

# where page.editors a prebuilt method that returns a sql statement. Where editors refers to the AdminUser class.
page.editors << new_admin  # displays all of the editors on page 1 on the index of the pages table

# the instance we made and it's attributes called "new_admin" gets appended an array 

# INSERT INTO `admin_users_pages` (`page_id`, `admin_user_id`) VALUES (1, 2)

#what this actually says is. to Insert the page_id of the page that we selected earlier which is one, and insert the admin_user_id of the new_admin that we created. then insert them into our admin_users_pages tables.
```

Some notes to remember

```ruby
page = Page.find(1) # shows the first page of the pages table
# page has_and_belongs_to_many :editors

#editors where we rename the AdminUser to
page.editors # displays all of the editors of page 1
# page.editors returns an array
[
  #<AdminUser id: 1, first_name: "Patrick", last_name: "Sunico", email: "PS@gmail.com", username:"PS", hashed_password: nil, created_at: "2016-07-07 06:16:14", updated_at: "2016-07-07 06:16:14">,
  
  #<AdminUser id: 2, first_name: "Jamal", last_name: "Dankerson", email: "JD@africa.com", username: "JD", hashed_password: nil, created_at: "2016-07-07 06:37:54", updated_at: "2016-07-07 06:41:37">
]
# create an instance of AdminUser, by finding an existing admin fomr the admin_users table
# editor has_and_belongs_to_many :pages
admin = AdminUser.find(1)

#admin Instance of the found AdminUser
admin.pages # displays all of the pages that are being managed by our editors inside an array

[
  #<Page id: 1, name: "Home Page", permalink: "home", position: 1, visible: true, created_at: "2016-07-07 05:58:47", updated_at: "2016-07-07 05:58:47">, 
  
  #<Page id: 2, name: "About Page", permalink: "about", position: 2, visible: true, created_at: "2016-07-07 05:59:12", updated_at: "2016-07-07 05:59:12">, 
  
  #<Page id: 3, name: "Contact", permalink: "contact", position: 3, visible: true, created_at: "2016-07-07 05:59:36", updated_at: "2016-07-07 05:59:36">
]

-----------------------------------------
  page.editors  	|  admin.pages
-----------------------------------------
 # AdminUser id: 1 	|  #Page id: 1 
 # AdminUser id: 1 	|  #Page id: 2
 # AdminUser id: 1 	|  #Page id: 3

 # AdminUser id: 2 	|  #Page id: 1
 # AdminUser id: 2 	|  #Page id: 2
 # AdminUser id: 2 	|  #Page id: 3
```

#### 7.4 Many-to-Many Associations: Rich

* Compared to many-to-many simple association
  * Still uses a join table, with two indexed foreign keys
  * Requires a primary key column.
  * Join table has its own model
  * No table name convention to follow
    * Names ending in "ments" or "ships" work well
* AdminUser - Section - SectionEdit
  * AdminUser has_many :section_edits.
    * found_user.section_edits
  * SectionEdit belongs_to :admin_user
    * section_edit.admin_users
  * Section has_many :section_edits
    * found_section.section_edits
  * SectionEdit belongs_to :section
    * section_edit.sections

In Ruby Syntax

```ruby
# 1. Inserting an AdminUser in section_edits table

# find a User in our existing AdminUsers Table
found_user = AdminUser.find(1) # returns "Patrick" from the admin_users Table

# From the instantiated AdminUser our variable "user" can now access the section_edits method/table

found_user.section_edits # returns an empty array since we haven't made an sql insert yet.

# Inserting a user to section_edits table
user.section_edits << user # appends the id of section to section_edits

# 2. Inserting a section in section_edits table

# find and exisiting user or create a new one

found_section = Section.find(1) # returns a "First Section" from the sections table

section.section_edits << found_section # appends the found section in the section_edits table
```

* Customer — Product — OrderShipment
  * Customer has_many :order_shipments	
    * new_customer.order_shipments

  * OrderShipment belongs_to :customers
    * order_shipment.customers

  * Product has_many :order_shipments
    * new_product.order_shipments

  * OrderShipment belongs_to :products
    * order_shipment.products



```ruby
# 1.Create a new Customer or Insert an existing one

new_customer = Customer.create(:customer_name => "Alfred", :address => "Brooklyn, New York")

# 2. Create a new OrderShipment
new_order = OrderShipment.create(:summary "Ordered an iPhone 6")

# 3. Append the new_order instance to this customer instance then save it to order_shipments table
new_customer.order_shipments << new_order

# 4. Create a new Product or Insert an Existing one
new_product = Product.create(:product_name => "iPhone 6")

# 5. Append the new_order instance, earlier to this product instance then save it to order_shipmens table
new_product.order_shipments << new_order

# OR Update the existing instance of new_order with the existing instance of new_customer, This will update an new customer to that specific instance we made earlier on OrderShipment Model
new_order.customers = new_customer
# In order to save the new append
new_order.save
```

Using Mass Assignment

```ruby
# find an existing customer
found_customer = Customer.find(1) # returns Alfred

# find an existing section
found_product = Product.find(1) # returns the first Product in the products table "iPhone 6"

SectionEdit.create(:customer => found_customer, :product => found_product, :summary => "Changes")

# to reload order_shipments array that is attained by our found_customer
found_customer.order_shipments(true)
```

#### 7.5 Traversing a Rich Association

* has_many :through
  * Treats a rich join like an has_and_belongs_to_many join
* Create a functional rich join first
* AdminUser — Section
  * AdminUser has_many :section_edits
  * AdminUser has_many :sections, :through => :section_edits
  * Section has_many :section_edits
  * Section has_many :admin_users, :through => :section_edits

```ruby
# Example 
# AdminUser 
class AdminUser < ActiveRecord::Base
  # AdminUser has_many section_edits;
  # many-to-many Rich
  has_many :section_edits

  # since we made a rich join already, AdminUser has_many sections inside our section_edits table
  has_many :sections, :through => :section_edits
end

# Section
class Section < ActiveRecord::Base
  # many-to-many Rich
  has_many :section_edits
  
  # many-to-many Rich :through
  has_many :editors, :through => :section_edits, :class_name => "AdminUser"
end

#SectionEdit (default from rich-joins)
class SectionEdit < ActiveRecord::Base
  # many-to-many association: Rich
  # Specify the name and it's class name, Since we changed the name from AdminUser to "editor" we need to set also the corresponding foreign key for this table, we defined a foreign key inside our migrations file
  # see the create_admin_users_pages_join.rb file
  belongs_to :editors, :class_name => "AdminUser" , :foreign_key => "admin_user_id"

  # Specify the name and it's corresponsing foreign key for this table
  belongs_to :section, :foreign_key => "section_id"
end
```

```ruby
# Find an Existing AdminUser

me = AdminUser.find(1)

# now we can access the sections table with out instance of AdminUser

me.sections # returns all of the sections manager by "me"

# Vice Versa, We can also find an existing Section that is being managed by AdminUser

section = Section.find(1) # returns "First Section"

# since we rename admin_users table to editors
section.editors # displays all of the editors of the section
```

### 8. CRUD Operations in Controllers

* Create 
* Read
* Update
* Delete


| Crud   | action  | concept                    |
| ------ | ------- | -------------------------- |
| create | new     | display new record form    |
|        | create  | process new record form    |
| read   | index   | list records               |
|        | show    | display a single record    |
| update | edit    | display edit record form   |
|        | update  | process edit recor         |
| delete | delete  | display delete record form |
|        | destroy | process delete record for  |



* Generate new Controllers
  * SubjectsControllers, PagesControllers, SectionsControllers
* Often: One controller per model; use plural names
* Allows for clear URS Path
  * subjects/new - Create
  * pages/new - Create
  * sections/new - Create

To Generate a Controller with specified action controllers and a views sub directory

```unix
rails generate "ControllerName" "action_name1" "action_name2" "action_name3" and so on.
```

#### 8.1 Read Action: Index

##### 8.1.1 Instance Methods

Where we transfer data from our controller to our view we can call instance methods something like in angular as a $scope where it binds the controller's data through the view altogether.

Where $scope is to Angular, Everytime a request comes in through our rails server rails makes an instance of our controller's classes then allows us to use instance variables from inside the class.

**SubjectsController**

```ruby
class SubjectsController
  def index # index action 
    @subjects = Subject.sorted # Class Query Method to be stored in a instance variable
  end
end
```

**index.html.erb**

```erb
# display the values in a table form
<div class="subjects_index">
  <h2>Subjects</h2>
  <!-- specify the route and append a html "class action" -->
  <%= link_to("Add new Subject", '#', :class => 'action new') %>
  
  <table class="listing" summary = "Subject list">
    <tr class="header">
      <th>&nbsp;</th>
      <th>Subject</th>
      <th>Visible</th>
      <th>Pages</th>
      <th>Actions</th>
    </tr>
  <!-- do an for each to display the subject in a row like fashion -->
  <!-- @subjects the instance variable we defined in our controller -->
  <% @subjects.each do |subject| %>
    <tr>
      <td><%= subject.position %></td>
      <td><%= subject.name %></td>
      <!-- Ternary operator, is subject visible yes, output yes or if subject.visible no, output no-->
      <td class="center"><%= subject.visible ? 'Yes' : 'No'%></td>

      <!-- defines the number of subjects in the pages table -->
      <td class="center"><%= subject.pages.size%></td>
      <td class="actions">

    <!-- specify the route and class action to show an corresponding record from the db -->
        <%= link_to("Show", '#', :class => 'action show')%>

    <!-- specify the route and class action to edit a corresponding record from the db -->
        <%= link_to("Edit", '#', :class => 'action edit')%>
      </td>
    </tr>
  <% end %> <!-- end of each loop -->
  </table> <!-- end of table-->
</div>
```

#### 8.2 Show Action: Show

##### SubjectsController

```ruby
# find a subject by it's param id from the url string ex. subject/show/:id => where :id is equal to the url id hash string
class SubjectsController 
	def show
      @subject = Subject.find(params[:id])
	end
end
```

##### show.erb.html

This will show the specific subject that we can define in our url string parameter, 

ex. subject/show/:id => where :id is equal to the url id hash string

```erb
<!-- from the index action we specified an instance variable called @subject 'singular' to display a specific subject through the view -->
<% link_to ("<< Back to List"), {:action => 'index'}, :class => 'back-link' %>

<div class="subjects show">
  <h2>Show Subject</h2>
  <table summary="Subject detail view">
    <tr>
      <th>Name</th>
      <td><%= @subject.name%></td>
    </tr>
    <tr>
      <th>Position</th>
      <td><%= @subject.position %></td>
    </tr>
    <tr>
      <th>Visible?</th>
      <td><%= @subject.visible ? 'true' : 'false' %></td>
    </tr>
    <tr>
      <th>Created</th>
      <td><%= @subject.created_at %></td>
    </tr>
    <tr>
      <th>Updated</th>
      <td><%= @subject.updated_at %></td>
    </tr>
  </table>
</div>
```



#### 8.3 Form Basics

#### Rails/ERB vs. HTML

* Any template code that can be written with Rails/ERB can also be written with simple HTML.
* Writing template code in Rails/ERB is almost always easier and more powerful than simple HTML.

##### 1. Basic Form Validation by assigning attribute values to attribute types

```erb
<!-- rails route to create a new subject /subjects/create/action-->
<form action = "/subjects/create" method="post">
  <input type = "text" name="name"/> <!-- create a new subject_name-->
  <input type = "text" name="position"/> <!-- assign a position-->
  <input type = "text" name="visible"/> <!-- assign if visible or invisible-->
  <input type ="submit" name="commit" value="Create Subject"/> <!-- submit the form-->
</form>
```

##### 2. HTML Form, array of parameters

```ruby
params[:subject] 
# {:name => "About Us", :position => '5', :visible => '1'} 
# the subject[:attr_type] we defined in the view can now be treated as an array of all the attribute values to create a new subject
subject = Subject.create(params[:subject])

```

where the object subject is the name of the instance of the class Subject from the model

```erb
<!-- rails route to create a new subject /subjects/create/action-->
<form action = "/subjects/create" method="post">
  <input type = "text" name="subject[name]"/> <!-- create a new subject_name-->
  <input type = "text" name="subject[position]"/> <!-- assign a position-->
  <input type = "text" name="subject[visible]"/> <!-- assign if visible or invisible-->
  <input type ="submit" name="commit" value="Create Subject"/> <!-- submit the form-->
</form>
```

##### 3. Rails/ERB form, array of parameters

```erb
# form_tag specify the :action for this form_tag helper for the create action
<%= form_tag(:action => 'create') do %>
  <%= text_field_tag('subject[name]') %>
  <%= text_field_tag('subject[position]')%>
  <%= text_field_tag('subject[visible]')%>
  <%= submit_tag("Create Subject")
<% end %>
```

##### 4. Rails Form, object-aware

```erb
# Short hand form in using the text_field helper methods for the create action form
<%= form_tag(:action => 'create') do %>
	<!-- Instead of using strings we can use symbols-->
	<%= text_field(:subject, :name) %>
  	<%= text_field(:subject,:position) %>
  	<%= text_field(:subject, :visible) %>
  	<%= submit_tag("Create Subject") %>
<% end %>
```

##### 5. Rails form, form_for :object(Most Commonly used)

```erb
<!-- form_form syntax -->

<%= form_for(:object_instantiated_from_class, :url => {:action => "action"}) do |var| %>
	<%= text_field(:attr_type) %>
<% end %>
-->

<%= form_for(:subject,:url => {:action => 'create'}) do |subject| %>
	<%= subject.text_field(:name) %>
	<%= subject.text_field(:position) %>
	<%= subject.text_field(:visible) %>
  	<%= submit_tag("Create Subject") %>
<% end %>
```

#### 8.4 New Action: new

* First answer doesn't need an instance variable to specified in this particular new action
* Second answer, and as a best practice, is to put an instance variable anyway

##### SubjectsController

```ruby
class SubjectsController
	def new
      @subject = Subject.new
      
      # we can also assign a default value as a placeholder for our form_helpers 
  	end
end
```

##### ERB File

```erb
# assemble the render template to be shown into the view/browser

<%= form_for(:subject,:url => {:action => 'create'}) do |subject| %>
	<%= subject.text_field(:name) %> 
	<%= subject.text_field(:position) %>
	<%= subject.text_field(:visible) %>
  	<%= submit_tag("Create Subject") %>
<% end %>
```

#### 8.5 Mass Assignment and Strong Parameters

* Rails term for passing a hash of values to an object to be assigned as attributes
* Example Subject.new(params[:subject])

##### Mass Assignment filtering

* Rails v1-2: Blacklisting of attributes in the model.


* Rails v3: Whitelisting of attributes, While attributes are blacklisted by default
* Rails v4: Strong parameters
* By default all values in the params hash are unavailable.

##### Strong Parameters

**Permits** an attribute to be mass assigned,  Whitelists the attributes inside of the params hash

```ruby
params.permit(:first_name,:last_name)
```

**Require** ensures that the parameters is present, If our attributes are assigned to subject we need to make sure that subject is present in the params, it also returns the value of the parameter.

```ruby
params.require(:subject) # returns :subject hash, similar to params[:subject]
```

**Working with Permits and Require**

```ruby
params.require(:subject).permit(:name, :position, :visible)
```

#### 8.6 Create Action: Create

* Instantiate a new object using the form parameters
* Then Save that object
* If save succeeds, redirect to the index action.
* If the save fails, redisplay the form so user can fix problems

##### SubjectsController

```ruby
class SubjectsController  
def create
    @subject = Subject.new(subject_params)
    if @subject.save # Save the object
      # If save succeeds, redirect to the index action
      redirect_to(:action => "index")
    else
      # If save fails, redisplay the form so user can fix problems render the new form_template
      render('new')
    end
  end
  private
   	  def subject_params
          params.require(:subject).permit(:name, :position, :visible)
      end
end
```

First we Instantiate a new object from the **"subject_param's"** form parameters, while whitelisting the attributes inside of the params hash

we made the **subect_params** method private so it **can't be called as an action**, then we append the name of this method to set a return value for the @subject instance inside the Subject.new as an instance

* same as using "params[:subject]", except that it:
  * raises an error if :subject is not present
  * allows listed attributes to be mass-assigned

#### 8.7 Create vs. Update

* params[:id]
  * New and create do not need an ID
  * Edit and update require's an ID and an existing record.
* Form Processing
  * **Create** uses new and save
  * **Update** uses find and update_attributes 

**SubjectsController**

* The edit action first finds the form based on our params then shows the form with the preexisting values from our database which then can be edited with using the update action
* The update action, is where we actually do the insert into the subjects table, Remember the code that we write here is automatically translated into a SQL format under the hood to do the actual insertion. 
  * Once the update is successful redirect the user back to the show page, which contains the following subject based on it's id.
  * if it fails re render the edit page again to make sure the user has no errors in input the fields

```ruby
  # edit action to show the form with the existing values
  def edit
    # find the subject to edit
    @subject = Subject.find(params[:id]) 
  end

  # edit + save action when user submits the form back again to the server
  def update
    # Find the existing Object using the form parameters
    @subject = Subject.find(params[:id])

    # update the values
    # the update_attributes automatically saves and inserts the new values back into the table without reinitializing a table row
    if @subject.update_attributes(subject_params)
        redirect_to(:action => "show" , :id => @subject.id)
      else
        render('edit')
    end
  end
```



**edit.html.erb**

* Here in our form_for helper we're specifying the action and the :id to be updated at.
  * The actual code from the text_field does not change since by default we are defining the text fields the preexisting values already, the only difference is the action itself.

```erb
<%= link_to("<< Back to List", {:action => "index"}, :class => "back-link") %>

<div class="subjects edit">
  <h2>Update Subject</h2>
  <!-- Specify the action and the id to be updated at -->
  <%= form_for(:subject, :url => {:action => "update", :id => @subject.id}) do |f| %>

    <table summary="Subject form fields">
      <tr>
        <th>Name</th>
        <td><%= f.text_field(:name) %></td>
      </tr>
      <tr>
        <th>Position</th>
        <td><%= f.text_field(:position) %></td>
      </tr>
      <tr>
        <th>Visible?</th>
        <td><%= f.text_field(:visible)%></td>
      </tr>
    </table>

    <div class="form-buttons">
      <%= submit_tag("Update Subject")%>
    </div>

  <% end %>
</div>
```

#### 8.8 Delete and Destroy Action

* **Delete** - Finds the form with an existing ID
* **Destroy** - is the process itself when it comes to delete the existing record.

**SubjectsController**

```ruby
# find the form be deleted
def delete 
  @subject = Subject.find(params[:id])
end

# The actual process of deletion
def destroy
  @subject = Subject.find(params[:id])
  @subject.destroy
  redirect_to(:action =>"index")
end
```

**delete and destroy action route ERB template**

```erb
<!-- delete.html.erb -->
<%= link_to("<< Back to List", {:action => "index"}, :class => "back-link") %>

<div class="subjects delete">
  <h2>Delete Subject</h2>

  <!-- Specify the action and the id to be deleted -->
  <%= form_for(:subject, :url => {:action => "destroy", :id => @subject.id}) do |f| %>
  <p>Are you sure you want to permanently delete this subject?</p>

  <p class="reference-name"><%= @subject.name %></p>

  <div class="form-button">
    <%= submit_tag("Delete Subject")%>
  </div>

<% end %>
</div>
```

##### 8.8.1 Flash Hash

* Gives the user a message for their actions made in a website
* Stores message in session data
* Clears old message after every request

**Flash Hash Definition**

```ruby
# Notice 
flash[:notice] = "The Subject was created successfully"

# Error
flash[:error] = "You don't have enough access privelages"
```



### 9.Rails Templates

##### 9.1 Layouts

We can define a preexisting layout that can be rendered among the actions and their views in their controllers.

```ruby
class Subjects 
  layout "admin"
  def index
  end
  
  def show 
  end
  
  def new 
  end
  ... and so on.
end
```

The basic code above illustrates that this controller and it's action will be rendered inside our admin layout defined in our /layouts folder. Meaning every route we typed in the browser within the Subjects Controller will be rendered inside of the layout admin.

**admin.html.erb** - Inside of our layouts folder

the container HTML that wraps around the yield tag will be the actual layout or shell of the action/templates we define separately. 

One thing to note about rail's rendering capabilities, rails does not render them sequentially, rails first gathers instance variables and then binds them to the template

```erb
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>Simple CMS | <%= @page_title || "Admin" %></title>
  </head>
  <body>
    <div id="header">
      <h1>Simple CMS Admin</h1>
    </div>

    <div id="main">
      <% if !flash[:notice].blank? %>
        <div class="notice">
          <%= flash[:notice] %>
        </div>
      <% end %>

      <div id="content">
        <!--before yield-->
        <%= yield %><!-- Where the templates get rendered-->
        <!--after yield-->
      </div>

    </div>

    <div id="footer">
      <p id="copyright">&copy; Patrick Sunico.</p>
    </div>

  </body>
</html>
```

##### 9.2 Partial Templates

Splitting up our actual controller templates into other partial templates to practice DRY CODE.

##### Definition

To render a partial we can use a render method wrapped around a preexisting template.

```erb
<%= render(:partial =>"file/to/partial", :locals => {:to_partial => value_passed } %>
```

Whenever we specify a partial and include it in an existing template, this will render the partial in between the render method from the preexisting template

##### subjects/new.index.html

```erb
<%= link_to("<< Back to List", {:action => "index"}, :class => "back-link") %>

<div class="subjects new">
  <h2>Create Subject</h2>
  <!-- form_for(subject/create) -->
  <%= form_for(:subject, :url => {:action => "create"}) do |f| %>
    <!-- the ":f" key is what the variable the partial will be using, while the "f" value is the value for the partial value -->
    <%= render(:partial => "subjects/partials/form", :locals => {:f => f}) %> <!--render-->
  <% end %>
</div>
```

##### subjects/edit.index.html

```erb
<%= link_to("<< Back to List", {:action => "index"}, :class => "back-link") %>

<div class="subjects new">
  <h2>Create Subject</h2>
  <!-- form_for(subject/create) -->
  <%= form_for(:subject, :url => {:action => "create"}) do |f| %>
    <!-- the ":f" key is what the variable the partial will be using, while the "f" value is the value for the partial value -->
    <%= render(:partial => "subjects/partials/form", :locals => {:f => f}) %> <!--render-->
  <% end %>
</div>
```

##### partials/_form.html

```erb
<table summary="Subject form fields">
  <tr>
    <th>Name</th>
    <td><%= f.text_field(:name) %></td>
  </tr>
  <tr>
    <th>Position</th>
    <td><%= f.text_field(:position) %></td>
  </tr>
  <tr>
    <th>Visible?</th>
    <td><%= f.text_field(:visible)%></td>
  </tr>
</table>

<div class="form-buttons">
  <%= submit_tag("Create Subject")%>
</div>
```

#### 10. Custom Helpers

* Ruby Modules
* created when generating a controller
  * File name and module name must correspond
* Helper methods are available in view templates
* Useful for:
  * Frequently used code
  * Storing complex code to simplify view templates
  * Writing large sections of ruby code

When Defining custom helpers first make sure if this particular helper will be shared among other controller views.

If so, we can define our custom helpers from within the application_helper.html.erb inside of our helpers folder.

* status_tag method that takes up 2 parameters, the boolean value from page.visible and the default parameters for the options hash
* now if boolean == true or visible show the content_tag with the span class of status true and append the double quotes from within the "<span>" "</span" tags, as a placeholder for text,
* whatever we defined inside of the options[:true_text] ||= "some string", will be outputted in the view in between the span tags

##### application_helper.rb

```ruby
module ApplicationHelper
  def status_tag(boolean , options = {})
    options[:true_text] ||= ''
    options[:true_text] ||= ''
    # OR
    # options[:true_text] ||= 'true' # outputs "true" if boolean is true within span tags with the class of status true
    # options[:false_text] ||= 'false' # outputs "false" if boolean is false within span tags with the class of status false

    if boolean 
      # if boolean = true
      content_tag(:span, options[:true_text], :class=> "status true") 
    else
      # if boolean = false
      content_tag(:span, options[:false_text], :class => "status false") 
    end
  end
end
```

##### index.html.erb

```erb
<!--if status(subject.visible = true) show the corresponding span tag and it's class--> 
<td class="center"><%= status_tag(subject.visible) %></td> 
```

#### 11. Sanitize Helpers

* Preventing Cross-site scripting
  * Prevents scripting events from our forms if sent via processing the form


* Escaping output
  * html_escape(), h()
  * raw()
  * html_safe - marks that html as safe
  * html_safe? - checks and see if that html is safe

##### strip_links(html) 

Removes HTML links from text

```erb
<!-- detects links and strips them out but the text itself is still rendered-->
strip_links('<strong>Please</strong> visit <a href="http://example.com'>us</a>')
```

##### strip_tags(html)

Removes all HTML tags from text

```erb
<!-- detects html tags and strips them out but normal text is still rendered-->
strip_tags('<strong>Please</strong> visit <a href"http://example.com">us</a>')
```

##### sanitize(html,options)

Removes html and javascript, watching for all tricks, 

Options: tags, :attributes(as arrays) to whitelist

```erb
sanitize(@subject.content, 
:tags => ['p','br','strong','em'], <!-- tags that can be used-->
:attributes => ['id', 'class', 'style']) <!--attributes that can be used with those tags-->
```

 



















#### . Validations

Data validation ensures that the data received from the form matches the attribute types we assigned in our model. 

To add validations presence and length validations to article model for title and description

```ruby
class Article < ActiveRecord::Base
  #validates if title is greater than 3 and less than or equal to 50
  validates :title, presence: true, length: {minimum: 3, maximum: 50} 
  
  #validates if title is greater than 10 and less than or equal to 300
  validates :description, presence: true, length: {minimum: 10, maximum: 300}
end
```

#### 8. URL Parameters

##### 8.1 Hash with parameters

Where action is the hello action, :page is which page we want to be directed at, :id as something that can be a reference id from a model.

```erb
<!-- Hash with Parameters -->
<!-- Where action is the hello action, :page is which page we want to be directed at, :id as something that can be reference from a model -->
<%= link_to('Hello with parameters', {:action => 'hello', :page => 5, :id => 20})%>

<!-- Will result as demo/hello?id=20&page=5 -->
```
##### 8.2 Reading Params from a controller 

**params** - Contains all GET and POST variables, Note that the hash it returns i.e is not a regular hash, But instead it is called a HashWithIndifferentAccess, It's a special kind hash in rails, that allows  us to reference the hash keys using either a symbol or a string

```ruby
#where :action hash key

#Symbol
params[:action]
#String
params["action"]
```

**Example**

```erb
<!-- Directs to hello.html.erb -->
<!-- link_to("Name of the link", "hash parameters") -->
<%=link_to('Hello with Parameters', {:action => 'hello', :page => 5, :id => 2, }) %>

<!-- Returns a hash string /demo/hello/2?page=5 -->
<!-- Where :id is a special hash that gets appended first in the hash string, following the other hash attributes -->
```

First we need to gather the parameters on the specific action route, then we can assign the values of the params individually and store them accordingly inside a instance variable.

```ruby
# demo_controller.rb

# To access what is inside of the browser's url link i.e
# if params = "demo/hello?id=20&page=5" where demo is the root, hello as the action, id=20 as the id, and page=5 is the page inside of the url params
# since params is a hash we can access it's values individually and assign them accordingly as instance variables inside of our controller

def hello 
  @action = params[:action]
  @id = params[:id].to_i # converted to integer 
  @pages = params[:page].to_i # converted to integer
end
```

**ERB Template**

Second from the ERB template we can interpolate the values of the params individually.

```erb
# hello.html.erb template
<!-- Output back the params from the controller -->
ACTION: <%= @action %>
ID : <%= @id %>
Next PAGE: <%= @page + 1 %>

<%= params.inspect %> <!-- returns a nice printed output of our params  -->
<!-- {"id" => "20", "page" => "5", "controller" => "demo", "action" => "hello"} -->
```

#### 9. Basic Route Types in Rails

* Simple route(Match route) 

In the long hand method, Where "demo/index" is the route, "demo#index" is the corresponding controller and action, and get as the action.

```ruby
#short hand method
get 'demo/index' # or
#Long hand method
match "demo/index", :to => "demo#index", :via=> :get
```

* Default route

```ruby
#Example Route
# GET /students/edit/52
###################################################
# REQUEST TYPE |     CONTROLLER    | ACTION | ID
#	  GET 	 	StudentsController   edit     52
###################################################
# Automatically inserts default action if not specified inside our StudentsController

# Not commonly used but commonly used for as a catch all route if no actions are given inside our application
match ':controller(/:action(/:id))', :via => :get

# Where students is the StudentsController, edit as the action the StudentController's Action, and 52 as an id of the student we want to edit.
```

* Root route - The default route when user requests for our webpage

```ruby
#root default route 
root 'index#actionController'
```

#### Some Other useful tips in Rails 

##### 1. link_to helper method

In ruby we can link several templates all together using these 3 methods.

```erb
# 1. Using href tags 
<a href="/demo/hello">Hello Page 1 using ahref</a>

# ERB Linking tag while specifying which action method to go to
<br><%= link_to('Hello Page 2 using link_to with specified action', {:action => 'hello'})%>

# ERB Linking tag while specifying on the uri path via rake routes
<br><%= link_to 'Hello Page using link_to with uri path', demo_hello_path %>

# Appending a class inside a link tag
<%= link_to "Hello Page using link_to with uri path", {:action => 'hello'}, :class %> "link-to-hello" %>
```

##### 2. YAML 

* YAML Ain't Markup Language
* used mainly for configurations similar to the jsconfig.js file in node 

##### 3. RAKE

* Rake is a simple ruby helper program.
* Runs scripts known as "tasks"

##### 4. Rails Environments

* Development Environment - used mainly, when we are developing our application.
* Production Environment - used mainly, when our application is staged to go out live for our user's to use.
* Testing Environment - used mainly, for testing, but without making changes to either our development and production environment, here we can do basic crud testing before implementing them in our development environment.


#### Useful git commands

**Redo Changes before commit**

```git
git checkout -f
```

**Making a Github Repo (with SSH Keys)**

**get ssh key from local machine**

```git
cat ~/.ssh/id_rsa.pub
```


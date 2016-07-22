### 1. Text Helpers and Number Helpers

##### 1.1 Text Helpers

- word_wrap
- simple_format
- simple_format + word_wrap
- truncate
- excerpt
- highlight
- pluralize
- cycle

```erb
<!-- text variable-->
<%=
  text = "Lorem ipsum dolor sit amet, consectetur adipisicing elit.\n Sunt iusto obcaecati sequi ex ut, maiores totam corrupti unde est, \n\n quidem quia adipisci quasi natus beatae nihil, suscipit consequatur dolor culpa?"
%>
```

#### word_wrap

If the text exceeds 30 characters in length it appends a "\n" at the end, However html does not respect the "\n" character so it won't be seen when we render the page.

```erb
<%= word_wrap(text, :line_width => 30) %> 
```

#### simple_format

Simple format respects line breaks, because for every "\n" it get's converted to <br> tags, and every "\n\n" gets converted into a new paragraph line "<p>"

```erb
<%= simple_format(text)%>
```

#### simple_format + word_wrap

A mixture of both word_wrap and simple_format,  convert a single "\n" to br tags, convert double "\n\n" as a new paragraph line, <br> then if character length exceeds 30 break a new line

```erb
<%= simple_format(word_wrap(text, :line_width => 30))%>
```

#### truncate

If character length exceed 40 show a … instead

```erb
<%= truncate(text, :length => 40, :omission => "...")%>
```

#### excerpt

excerpt is the same as truncate but looks for a target word, When excerpt finds the target word, excerpt will snip out text 7 characters before the target word and 7 characters after the target word.

```erb
<%=excerpt(text, 'corrupti', :radius => 7, :omission => "...")%>
```

#### highlight

highlights the target text as specified within single quotes 

```erb
<%= highlight(text, 'Lorem ipsum')%>
```

#### highlight multiple targets

highlights multiple targets inside of an array

```erb
<%= highlight (text, ['consectetur', 'adipisicng'], :highlighter => '<em>\1</em>')%>
```

#### pluralize

pluralize is helpful when were working with database tables, It basically pluralizes a word or  variable from a singular noun to a plural noun, This is extremely useful if were searching and querying something from a database and tell something to the user i.e "I found 1 product" or "You have 2 messages"

```erb
<%= student.pluralize %> <!--students-->
<%= ox.pluralize %> <!--oxen-->

<% [0,1,20].each do |n| %>
  <!-- if n = 0, stay singular -->
  <!-- if n = 1, still singular -->
  <!-- if n = 2 or more, pluralize -->
  <ul>
    <li><%= pluralize(n, 'product')%></li>
    <li><%= pluralize(n, 'octopus')%></li>
    <!-- predefined singular and plural form -->
    <!-- if n = 1 -> 1 snipe -->
    <!-- if n = 2 -> 2 snope -->
    <li><%= pluralize(n, 'snipe','snope')%></li>
  </ul>
<% end %>
```

#### cycle

cycles trough the given values x number of times

```erb
<% 5.times do%>
	<%= cycle('red','green','blue')%> <!-- red, green, blue, red, green-->
<% end %>

<!--OR-->

<!--cycle through class names-->

<div class="<%= cycle("visible","invisible")%>">
  <li>Visible</li>	
  <!--Invisible-->
  <li>Visible</li>	
</div>
```

##### 1.2 Number Helpers

- number_to_currency
- number_to_percentage
- number_with_precision / number_to_rounded
- number_with_delimiter / number_to_delimited
- number_to_human
- number_to_human_size
- number_to_phone

##### Number Helper Options

- :delimiter
  - Delimits thousans; default is ","
- :separator
  - Decimal separator; default "."
- :precision
  - Decimal places to show; default varies (2-3)

##### number_to_currency

```erb
number_to_currency(34.5)<!--returns $34.50-->
number_to_currency(34.5, :precision => 0, :unit => "gbp", :format => "%n %u")
<!-- £34.50 -->
<!--where precision is the decimal places to show-->
<!-- :unit as the type of currency -->
<!--:format as "%u" as the unit which comes first, then "%n" as the number which comes last-->
```

##### number_to_percentage

```erb
number_to_percentage(34.5) <!-- returns 34.500%-->
<!-- appends the "," mark in between the whole number and the first decimal-->
number_to_percentage(34.5, :precision => 1, :separator => ',') <!-- 34,5% -->
```

##### number_with_precision()

```erb
<!--round off-->
number_with_precision(34.56789)
<!-- 34.568 -->

<!-- displays 6 decimal numbers at the end of the whole number if less than 6 append 0-->
number_with_precision(34.56789, :precision => 6)
<!--34.567890-->
```

##### number_with_delimiter

```erb
<!--Basically a thousands separator-->
number_with_delimiter(3456789)
<!--3,456,789-->

<!-- define the delimiter as spaces-->
number_with_delimieter(345678, :delimiter => ' ')
<!--3 456 789--> 
```

##### number_to_human

```erb
<!-- Transcibes numbers into human readable form-->
number_to_human(123456789)
<!--123 Million-->

<!-- Transcribe the number into human readble form rounded off -->
number_to_human(123456789, :precision => 5)
<!-- 123.46 Million-->
```

##### number_to_human_size

```erb
<!-- Transcribes numbers into data measurements-->
<!--1234567 as kb-->
number_to_human_size(1234567)
<!-- 1.18 MB-->

number_to_human_size(1234567, :precision => 2)
<!-- 1.2 MB -->
```

##### number_to_phone

```ruby
number_to_phone(1234567890)
<!--123-456-7890-->

number_to_phone(1234567890,
  :area_code => true # put "()" around the area code
  :delimiter => ' ',
  :country_code => 1,
  :extension => '321'
)
# +1 (123) 456 7890 X 321
```

### 2. Date and Time Helpers

##### 2.1 DateTime calculations using integers

* second/seconds
* minute/minuts
* hour/hours
* day/days
* week/weeks
* month/months
* year/years



##### Datetime calculations from Time.now

* ago


* from_now

```ruby
# From this date + 30 days and subtract 23 minutes 
Time.now + 30.days - 23.minutes

# The same algorithm, however 30 days from now(increment 30 from this day - 23 minutes)
30.days.from_now - 23.minutes
```

##### Relative DateTime calculations

* beginning_of_day / end_of_day - AM/PM
* beginning_of_week / end_of_week - Monday/ Sunday
* beginning_of_month / end_of_month - January/December
* beginning_of_year / end_of_year - 
* yesterday / tomorrow
* last_week / next_week
* last_month / next_month
* last_year / next_year

```ruby
# From this day calculate the seconds of last year as a starting point, and it' last month(December), from the beginning of time.

# December 2015 1,

Time.now.last_year.end_of_month.beginning_of_day
```

##### Ruby DateTime formatting

##### strftime Format Codes

* Day	
  * %a - The abbreviated weekday name ("Sun")
  * %A - The full weekday name ("Sunday")
* Date 
  * %d - Day of the month(01..31)
* Month
  * %b - The abbreviated month name("Jan"), ("Dec")
  * %B - The full month name ("January")
  * %m - Month of the year in numeric form (01...12)
* Year
  * %y - Year without a century (00..99) - century = 100 years
  * %Y - Year with century
* Time 
  * %H - Hour of the day, 24 hour clock(00..23) Military time
  * %I - Hour of the day, 12-hour clock (01..12) Standard Tie
  * %M - Minute of hour(00..59) 0 - 59 minutes = 1 hour
  * %S - Second of the minute (00..60)
  * %p - Meridian indicatir ("AM" or "PM")
  * %Z - Time zone name

```ruby
# Implementation
datetime.strftime(format_string)

# Example 
# Displays from this time
# July 13, 2016 11:28, AM
Time.now.strftime("%B %d , %Y %H:%M", "%p")
```

##### Rails DateTime Formatting

```ruby

datetime.to_s(format_symbol)

#Some Default Formats
# :db "2016-07-13 11:35:18" datetime.to_s(:db)
# :number "20160713113518" datetime.to_s(:number)
# :time "11:35"datetime.to_s(:time)
# :short "09 Jan 13:36" datetime.to_s(:short)
# :long "January 09, 2013 13:36" datetime.to_s(:long)
# :long_ordinal "January 9th 2013 13:36" datetime.to_s(:long_ordinal)

# Custom Formats can be defined inside of 
# config/initializers/date_formats.rb
# DATE_FORMATS = Constant
Time::DATE_FORMATS[:custom] = "%B %e, %Y at %l:%M %p"
```

### 3. Custom Helpers

- Ruby Modules
- created when generating a controller
  - File name and module name must correspond
- Helper methods are available in view templates
- Useful for:
  - Frequently used code
  - Storing complex code to simplify view templates
  - Writing large sections of ruby code

When Defining custom helpers first make sure if this particular helper will be shared among other controller views.

If so, we can define our custom helpers from within the application_helper.html.erb inside of our helpers folder.

- status_tag method that takes up 2 parameters, the boolean value from page.visible and the default parameters for the options hash
- now if boolean == true or visible show the content_tag with the span class of status true and append the double quotes from within the "<span>" "</span" tags, as a placeholder for text,
- whatever we defined inside of the options[:true_text] ||= "some string", will be outputted in the view in between the span tags

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

### 4. Sanitize Helpers

- Preventing Cross-site scripting
  - Prevents scripting events from our forms if sent via processing the form


- Escaping output
  - html_escape(), h()
  - raw()
  - html_safe - marks that html as safe
  - html_safe? - checks and see if that html is safe

##### Escaping output

```erb
<% evil_string = "<script>alert('Gotcha!');</script>" %>
<% good_string = "<strong> Welcome to my site </strong>" %>

<!-- By default rails detects the script tags and html tags and refuses to render these strings -->
<!-- renders them as normal strings -->
<%= evil_string %> <br>
<%= good_string %> <br>

<!-- processes the strings defined as normal html string (Caution) -->
<!-- renders the strings as html -->
<%= raw(evil_string) %> <br>
<%= raw(good_string) %> <br>

<!-- forces all of the html as safe -->
<%= evil_string.html_safe %> <br>
<%= evil_string.html_safe %> <br>


<!-- checks if the strings are safe -->
<!-- returns true if safe, returns false if not safe -->
<%= evil_string.html_safe? %> <br>
<%= good_string.html_safe? %> <br>
```

##### strip_links(html)

Removes HTML links from text

```erb
<!-- detects links and strips them out but the text itself is still rendered-->
strip_links('<strong>Please</strong> visit <a href="http://example.com'>us</a>')                      
                                                    
<%= strip_links(simple_string) %>
```

##### strip_tags(html)

Removes all HTML tags from text

```erb
<!-- detects html tags and strips them out but normal text is still rendered-->
strip_tags('<strong>Please</strong> visit <a href"http://example.com">us</a>')


<!-- checks the string if there any containing links or tags then strips them with the strip_links and strip_tag helper methods-->
<% simple_string = '<strong> Please </strong> visit <a href="http://example.com"> us </a>'%>

<%= strip_tags(simple_string) %> <br>

```

##### sanitize(html,options)

Removes html and javascript, watching for all tricks, 

Options: tags, :attributes(as arrays) to whitelist

```erb
sanitize(@subject.content, 
:tags => ['p','br','strong','em'], <!-- tags that can be used-->
:attributes => ['id', 'class', 'style']) <!--attributes that can be used with those tags-->

<!-- by default sanitize will just remove javascript only -->
<%= sanitize(evil_string) %> <br>

<!-- However if we specify a tags key or an attributes key we can specify which tags and attributes are available to be rendered for the view or be inserted into the db -->
<%= sanitize(simple_string, :tags => ['strong', 'em', 'a']) %> <br>

<!-- to specify no tags or links to whitelist just leave the tags as a blank array -->
<%= sanitize(simple_string, :tags => []) %> <br>
```

### 5. Images

- Location 
  - With asset pipeline: /app/assets/images
  - Without asset pipeline: /public/images
    - User-uploaded images: /public/images
  - Image Upload Gems
    - Paperclip, CarrierWave

#### Image Helpers

```erb
<%= image_tag('logo.png')%>
<!-- image_tag with default size and alt name-->
<%= image_tag('logo.png', :size => '90x55', alt: => 'logo')%>

<!--image_tag with default width and height-->
<%= image_tag('logo.png', :width => 90,:height => 55)%>
```

### 6. Forms

#### Rule of Thumb using form_for

- Take note form helpers automatically creates an html id specific to it's table name as we defined on our schema or in our migration file.

```erb
<%= form_for(:instance_from_class, :url => {:action => "action"}) do|var|%>
    <%= var.text_field(:attr_type) %>
<% end %>
```

#### Rails/ERB vs. HTML

- Any template code that can be written with Rails/ERB can also be written with simple HTML.
- Writing template code in Rails/ERB is almost always easier and more powerful than simple HTML.

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
```

```erb
<%= form_for(:subject,:url => {:action => 'create'}) do |subject| %>
	<%= subject.text_field(:name) %>
	<%= subject.text_field(:position) %>
	<%= subject.text_field(:visible) %>
  	<%= submit_tag("Create Subject") %>
<% end %>
```

#### 6.1 Form Option Helpers

##### Form Helpers Options

- text_field
- password_field
- text_area
- hidden_field
- radio_button
- check_box
- file_field
- label

##### A Basic form featuring all of the form helpers listed above

```erb
<%= form_for(:subject, :url => {:action => "create"}) do |f| %>
  <%= f.text_field(:name, :size => 40, :maxlength => 40) %>
  <%= f.password_field(:password, :size => 40, :maxlength => 40 %>
  <%= f.text_area(:description, :size => "40x5" %>
  <%= f.hidden_field(:token) %>
  <%= f.radio_button(:content_type, "Ruby"%>
  <%= f.radion_button(:content_type, "JavaScript"%>
  <%= f.label(:visible) %>
  <%= f.check_box(:visible)%>
  <%= f.file_field(:logo) %>
  submit_tag("Submit")
<% end %>
```

##### Using select tag only

```erb
<%= select(object,attribute,choices, options ={}, html_options={}) %>
```

##### Options

- :selected => object.attribute select other than the object's attribute value, i.e subject.id, subject.name and etc.
- :include_blank => false // blank option
  - :prompt => false//text prompt option
- :disable => false 

##### Select Helper (drop-down list)

```erb
<%= form_for(:section, :url => {:action => "create"}) do |f| %>
<!--Range Selection-->
<%=f.select(:position, 1..5) %>

<!--Array Selection-->
<%=f.select(:content_type, ['text', 'HTML']) %>

<!-- Hash Selection-->
<!--if table is boolean give visible 1 and hidden 0-->
<%=f.select(:visible, {"visible" => 1, "hidden" => 2}) %>

<!--Array of arrays-->
<!-- Selects all of the pages and "maps" it inside an array from inside the blog here we can specify what type of values we want from the object with p as a reference from Page and name and ias the attribute type -->

<!-- Map a Class Objects instances by itself itself-->
<%= f.select(:page_id, Page.all.map {|p| [p.name, p.id]})%>

	submit_tag("Submit")
<% end %>

<!-- or from the controller pass an instance variable-->
<%= f.select(:page_id, @pages.map {|p| [p.name, p.id]})%>

	submit_tag("Submit")
<% end %>
```

#### 6.2 Date Select Helper

Common Helpers: 

* :start_year => Time.now.year-5 - Determines the range from start 
* :end_year => Time.now.year+5 - Determines the range from end
* :order => [:year, :month, :day] - Orders dates in Y, MM , DD
* discard_year => false - Discards the year
* :discard_month => false - Discard the month
* :discard_day => false - Discards the day
* :include_blank => false - Includes a blank selection if user selected none
* :prompt => false - prompts a user for a date
* use_month_numbers => false - configured to use month as numbers instead of names
* :add_month_numbers => false 
* :use_short_month => false 
* :date_seperator => "" - Adds space between the dates 

```ruby
date_select(:object, :attribute, options={}, html_options={})
```



#### 6.3 Time Select Helper

* :include_seconds => false - does not include seconds only HH:MM
* :minute_step => 1 - displays 00 as the beginning all the way to 59 which are seconds in a minute
* :prompt => false - prompts a user for a Time input
* :time_separator => ":" - includes a time separator between the time values

```ruby
time_select(:object, :attribute, :options={}, html_options={})
```



#### 6.4 Date and Time Select Helper

* all date_select and time_select options 
* :datetime_separator => "-"

```ruby
datetime_select(object, attribute, options={}, html_options={})
```

#### 7. Handling Form Errors

##### Simple Validation

* validates_presence_of:name - Requires a form to have some value for an attribute, or else rails won't allow it to be saved to the database.
* object.errors - Array containing any errors added by validations
  * Remember when has an error in inputting some values into forms, we will rerender the form again to let the user fix their errors. so if the save fails don't save the value but instead rerender the form to let the user resubmit those values.

##### Useful methods for errors

* object.errors.clear - Forcibly clear the errors at that point
* object.errors.size - Tells how many errors there are.
* object.errors.each {|attr,msg| … } 
  * Example: returns a message :name, "can't be Blank"
* object.errors.full_messages.each{|msg| … }(Commonly used)
  * Example: returns a message "Name can't be blank"

##### Displaying errors

* List errors above the form
* Print/Highlight errors with each form input

#### 8. Preventing Cross-Site Request Forgery

* CSRF for short
* Require User Authentication
* Regulary log out inactive users
* GET requests should be read-only
* Actions that expect POST request should only respond to POST request



##### 8.1 Authenticity token

Unique token in a users session file. - 

When the user views a form to create, update, or destroy a resource, the Rails app creates a random `authenticity_token`, stores this token in the session, and places it in a hidden field in the form. When the user submits the form, Rails looks for the `authenticity_token`, compares it to the one stored in the session, and if they match the request is allowed to continue.

```
{"utf8"=>"✓", "authenticity_token"=>"CZ34cxfkD+IafDTx2x5Prp/73IS3DHO5d7BpIN3Sj/7SCINIV4LF8YlqV48zSv2pD3Gr10STe3eKI6/LQBcEwA==", "page"=>{"subject_id"=>"3", "name"=>"First Page", "permalink"=>"first", "position"=>"1", "visible"=>"1"}, "commit"=>"Update Page", "controller"=>"pages", "action"=>"update", "id"=>"1"}
```



```ruby
# Validation Method in prevent CSRF Attacks
protect_from_forgery with: :exception
```



#### CSRF Tokens for JavaScript and AJAX

Use a meta tag to let JavaSript and AJAX follow the validation method that is defined in our application  controller. In which case it prevents all CSRF attacks either from JavaScript or AJAX

```erb
<%= csrf_meta_tags %>
```




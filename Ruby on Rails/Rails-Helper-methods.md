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






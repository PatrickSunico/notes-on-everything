### 12. Authentication

- Password-protected areas are common in most web apps.
- Examining best practices will help avoid costly mistakes
- Development choices and security concerns are intertwined topics.

##### Topics and features to discuss

- Encrytion techniques
- Non-database attributes
- Cookies and sessions
- Controller method accessibility
- Before actions

##### Database and the Ticket Analogy

- Admin creates a user in the database - Purchase tickets for a concert
- User logs in via a login form - Wait in line to pick uo tickets
- Application authenticates user - Presents ID and get tickets, Get hand stamp and enter
- User requests additional password-protected pages. - Show hand stamp, avoid line and re-enter.
- User logs out - Wash away hand stamp

##### Authentication step-by-step

1. Admin creates a user in the database
   - Password is encrypted before user is stored
2. User logs in via a login form
3. Application authenticates user
   - Searches for username in database
   - If username found: compares with encrypted password.
   - If password matches: sets a variable in the session to the user ID and redirects back to a post-login page
4. User requests additional password-protected pages
   - Cookies and session data are available with each request.
5. Application checks the session data for the user ID
   - If present: returns the requested page.
   - If absent redirects to the login form.
6. User logs out
   - Set user ID stored in session variable to NULL.

##### Hashing passwords

- One-way encryption - Means non reversible, even by us
  - Same inputs(password) + same hashing algorithm = same output
  - Actual password gets encrypted, then stored.
  - Attempted password gets encrypted, compared against stored.
- Blowfish 
  - High level of security
  - Public domain, no patents, free to use
  - Blowfish is Slow, Slow enough to prevent or prolong a brute force attack on your server.

##### Securing Passwords in Rails

Drop this line inside an Active Record Model that enables a lot of best practices and functionality when it comes to securing passwords in rails.

```ruby
has_secured_password
```

##### Prerequisites

1. Application must have bcrypt-ruby gem installed
2. Table must have a string column for "password_digest"
   - Create a separate migration file to add a column for either a User entity or an Admin Entity

##### Setting up bcrypt and a basic authentication in our application

First, We need to make a separate migration file that will add a column header that we can name as password_digest, That will store in the unique encrypted password hash inside of each users in the table. OR We can also define it inside the AdminUsers table. 

We can name the migration file anything we want, But for this case I've named it as "AddPasswordDigestToAdminUsers"

```ruby
class AddPassWordDigestToAdminUsers < ActiveRecord::Migration
  def up
    # Adds a column header inside our admin_users table
    add_column "admin_users", "password_digest", :string
  end

  def down
    # To Rollback to previous migration
    remove_column "admin_users", "password_digest", :string
  end
end
```



Next is to either bundle gem install bcrypt that is already defined in our gemfile all we need to do is uncomment it then do a bundle install.

Lastly is to drop this line of code in our AdminUsers ActiveModel in our rails app.

```ruby
has_secure_password
```

with this we now have a lot of features that can be used for best practices in securing our application.



has_secure_password - Adds a virtual attribute for a password, which does not exist in the database and When we pass in an unencrpyted_password to password, It's going to behind the scenes, encrypt it with blowfish and store it in our password digest field

##### Adds in validation methods for you automatically.

```ruby
# reader method that does not exist in our database.
attr_reader :password

# A unencrypted password gets passed in and encrypts with blowfish
"#password=(unencrypted_password)"

# A user must always have a password inputted inside a form field
validates_presence_of :password, :on => :create

# validates password_confirmation if the confirmation matches the password
validates_presence_of :password_confirmation 
validates_confirmation_of :password

# pass in a plain text password to begin, and decides whether or not that this password has a match in our database, and it'll either render a page for the user or it'll return false.
'#authenticate(unencrypted_password)'
```

##### 12.1 Login and Logout forms 

In order for our users to sign in or signup we have to make a login or signup form. So we can authenticate them if a user exists in a database or is willing to create a new one.

##### Basic Login Form

##### From our generated controller "AccessController"

```ruby
class AccessController < ApplicationController
  layout 'admin'
  def index
  end

  def login
  end

  def attempt_login
    # Check if the params[:username] and params[:password] hash contains a value
    if params[:username].present? && params[:password].present?
      found_user = AdminUser.where(:username => params[:username]).first
      if found_user
        authorized_user = found_user.authenticate(params[:password])
      end
    end

    if authorized_user
        # todo: mark user as logged in
        flash[:notice] = "You are now logged in"
        redirect_to(:action => "index")
    else
      flash[:notice] = "Invalid username/password combination"
      redirect_to(:action => 'login')
    end
  end

  def logout
    # Marks a user as logout
    flash[:notice] = "You have logged out"
    redirect_to(:action => "login")
  end
end

```

Let's review some code, 

- The **index** method is the end point on which if the user succeeds in logging in or signup this is where we want our user to redirected to. In short this is the master page.

##### access/index action

```ruby
  def index
    # display text & links
  end
```

##### access/index view

```erb
<% @page_title = "Admin Menu" %>
<div class="menu">
  <h2>Admin Menu</h2>
  <div class="identity">Logged in as: </div>
  <ul>
<!-- specify which link path the user can go to, In this case we want to go to subjects/index since index is a root route we can just call the controller /subjects -->
    <li><%= link_to("Manage Subjects", :controller => "subjects")%></li>
    <li><%=link_to("Manage Pages", :controller => "pages")%></li>
    <li><%=link_to("Manage Sections", :controller => "sections")%></li>

    <li><%=link_to("Manage Admin Users",'#')%></li>
    <li><%= link_to("Logout", :action => 'logout')%></li>
  </ul>
</div>
```

- The **login** method, is the starting point on which a user can sign in or sign up first in order to reach the index method, this displays the form and on user submit, submits it to our **attempt_login method**

##### access/login action

```ruby
  def login
    # displays login form
    # On user submit redirect to attempt_login to check user authentication
  end
```

##### access/login view 

```erb
<% @page_title = "Admin Login"%>
<div class="login">
  <!-- On Submit, Submit the values through our attempt_login action -->
  <%= form_tag(:action => "attempt_login") do%>
    <table>
      <tr>
        <td><%= label_tag(:username) %></td>
        <td><%= text_field_tag(:username)%></td>
      </tr>

      <tr>
     <!-- password as a virtual attribute type that get encrypted with blowfish -->
        <td> <%= label_tag(:password)%>
        <td><%= password_field_tag(:password)%></td>
      </tr>

      <tr>
        <td>&nbsp;</td>
        <td><%= submit_tag("Log in")%></td>
      </tr>
    </table>
  <% end %>
</div>
```

- The **attempt_login** method, is the method that decides whether the user is authorized to view or be redirected to the **index** page.

```ruby
  def attempt_login
    # Check if the params[:username] and params[:password] hash contains a value
    if params[:username].present? && params[:password].present?
      # Next find the user that matches with the submitted user from the params[:username] hash, then store that into our found_user variable
      found_user = AdminUser.where(:username => params[:username]).first
      if found_user
        # use the found_user variable as reference to where we matched the user from our db and from the params hash.

        # the authenticate method is built in with the has_secure_password Class Method
        # authenticate the password from the params[:password] hash
        authorized_user = found_user.authenticate(params[:password])
      end
    end

    # if authorized user is correct redirect them to the page
    if authorized_user
        # todo: mark user as logged in
        flash[:notice] = "You are now logged in"
        redirect_to(:action => "index")
    else
      flash[:notice] = "Invalid username/password combination"
      redirect_to(:action => 'login')
    end
  end
```

- The **logout** method, is the method the basically states that a user has logged out and ended his /her session in our web application.

```ruby
  def logout
    # Marks a user as logout
    flash[:notice] = "You have logged out"
    redirect_to(:action => "login")
  end
```

##### 12.2 Cookies and Sessions

##### Cookies

- Adds "state" on a users interaction with our website
- Web server sends data in a cookie file to the browser, which then saves it.
- Browser sends cookie data with each request for that web server.

##### Limitations

- Have a maximum size of 4K (equal to 4096 characters)
- Reside on the user's computer
  - Can be easily deleted, read or altered.

##### Cookies in Rails

```ruby
cookies[:username] = "jsmith" # stores a user value of john smith
#value of johnsmith that is set to expire in a week.
cookies[:username] = {
  :value => "jsmith", :expires => 1.week.from_now 
}
```

##### erb

```erb
<!-- reading values from a cookie hash -->
<%= cookies[:username]%>
```

##### Sessions

- Designed to address the limitations of cookies
- Web server sends an ID in a cookie to the browser, which then saves it.
- Browser sends cookie data with each request for that web server.
- Web server or application uses cookie ID to locate a session file.

##### Limitations

- Extra time to retrieve session file for each request
- Old session files accumulate
- Session cookie still resides on the user's computer

##### Sessions in Rails

```ruby
session[:username] = "jsmith"

```

##### erb

```erb
<%= session[:username] %>
```

##### Session Storage

- File storage
  - Default in Rails v1, removed now
  - Slow does not scale, file system bloat
- Database storage
  - Default in Rails v2, moved to a RubyGem
  - Not fast, requires database call, database bloat
- Cookie storage
  - Default in Rails v3-4, "super cookie"
  - No database or file system bloat since this cookie is being stored in the user's cookie
  - The user's session cookie is Encoded, not encrypted
  - Digest value stored to prevent tampering
  - Fast, 4k Maximum size, no bloat, no hijacking

##### 12.3 Restricting Access with before_action

##### before_action

Allows a user to perform a task, before an action is executed, 

* Specify which methods activate the before_action
  * :only => [:method1, :method3]
  * :except => [:method2]
* Should call private or protected methods
  * So They can't be called as action
* Must return false to halt before_action chain

##### Usage and Syntax

```ruby
class AccessController < ApplicationController
  before_action :confirm_logged_in, :except => [:login, :attempt_login, :logout]
  
  private 
  # so only this class can call it, If we want this globally defined in inside the ApplicationController
  # defined in private so we know it's not a action/route that are not accessible 	for people calling different action routes
  def confirm_logged_in
      unless session[:user_id] # = unless = false or 0
      flash[:notice] = "Please log in."
      redirect_to(:action => 'login')
      return false # halts the before action
      # else return true, then carry on towards the index page
    end
  end

end
```

To Further explain before_actions in User Authentication, 

* **First** we defined a private method called **confirmed_log_in**, that states unless a user's session cookie with a value of it's user id = 0 or false. Then do a flash notice and then redirect back to the login page. **note on unless, unless will only work if the value to be described is false.**
* **Second**, in order to activate our private method, We must first set it as a before action. **A before action** is best describe as a request halter, meaning in authentication before a user can visit index or any parts of the site they must first be logged in or authenticated.

```ruby
# before any action confirm logged in first then exclude actions so users can login, and submit those values.
before_action :confirm_logged_in, :except => [:login, :attempt_login, :logout]
```

* **Third**, We exclude specific actions so the user has some privileges in logging in, requesting a login attempt and as well as logout but can be excluded in the except options since, the user is **logged out** anyway.
* With those actions exempted, the actions we defined inside as a except option, will now process whether the user is an existing user or not. Once the user successfully logs in they can access parts of the page with their exact privileges.
* A best practice is to defined this confirm_logged_in method inside our ApplicationController, That way other controllers can have proper security and authorization rights when it comes to visiting other parts of the site.

**In summary, We can say that every page is halted by our before_action and it's private method, except for the only three actions we excluded**

##### 12.4 Using before_action and the 'only' option

Basic Syntax

```ruby
before_action :method_name, :only => [:method1,:method2,:method3]
```

With only as an option, this is similar to the :except option, except it only halts the actions a user can do only as a defined method inside of the array we can see above.

##### Example:

We have a SubjectsController that has 7 methods,

```ruby
class SubjectsController < ApplicationController
  # Inherits :find_subject method from only on these instance methods/action routes
  before_action :find_subject, :only => [:show, :edit, :update,:delete, :destroy]
  def edit
    # find the subject to edit using find_subject method
    # without before_action
    @subject = Subject.find(params[:id])
    #with before_action we can instead remove the first line of code
  end

  def update
  end

  def delete
    # the same goes without before_action in new action
    # or without before_action 
  end

  def destroy
    #with before_action we can instead remove the first line of code
    subject_name = @subject.name
    @subject.destroy
    flash[:notice] = "Subject '#{subject_name}' Deleted successfully"
    redirect_to(:action => "index")
  end

  private
  # Finds the subject by it's parameter's id
  def find_subject
    @subject = Subject.find(params[:id])
  end
end
```






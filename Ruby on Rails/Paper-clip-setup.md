# Setting up Paperclip for image or file uploads

#### Pre-requisites 

* ImageMagick - Handles the image processing

  ```unix
  brew install imagemagick
  ```


* GhostScript(Optional) - Handles pdf files

#### 1. Generate a basic scaffold including the table columns

For a quick start up make a generated scaffold then name the scaffold whatever you want.
For this purpose and simplicity let's call it Post. and specify the table columns for the table.

```unix
rails generate scaffold Post title:string content:text
```

Create the database 

```terminal
#create the database and the schema
rake db:create
#then do migration
rake db:migrate
```

#### 2. Install the gems

Install the paperclip gem and place it onto you gem file

```ruby
gem "paperclip", "~> 5.0.0"
```

Then do a bundle install then you're set up.

#### 3. Setting up the model 

Third, we need to setup the model for our Post model to be associated with the paper clip helper method.

```ruby
# Note name the attached file whatever but for naming convention name it simply as as an image.
class Post < ActiveRecord::Base
  has_attached_file :image, styles{large: "800x800>", medium: "600x600>", thumb: "300x300#"}
  # validates an image to be uploaded if the particular image is in the correct format
  validates_attachment_content_type :image, content_type: /\Aimage\/.*\Z/
end
```

#### 4. Setting up the migration file

Note we can set this up manually before we do the initial rake db:create to create the database, or the preffered way is to use, rails migration generator:

```terminal
rails generate paperclip "main_model" "name of attached file"
```

With post and image as our example we can do this like so:

```teminal
rails generate paperclip post image
```

this will generate a new migration file called `AddImageColumnsToPost` 

```ruby
class AddAttachmentImageToPosts < ActiveRecord::Migration
  def self.up
    add_attachment :posts ,:image
  end
  def self.down
    remove_attachment :posts, :image
  end
end
```

#### 5. Setup the mass assignment permits for the image

So if a user want's to create a new post along with a image upload we need to whitelist the image hash attribute for mass assignment whenever a user posts in our create route.

```ruby
# strong_parameters whitelisting the image hash
def user_params
  # Since we have 2 other attribute types we can add them in also
  params.require(:posts).permit(:title,:content,:avatar)
end
```

#### 6. Setting up the view

We can use a image_tag helper to bind the images to the view and render them through the browser. If we have a .for each inside of our erb template we can simply just put this through there. then display all of the existing images inside our public file or that was uploaded.

```erb
<%= image_tag @post.image.url %> <!--Specifies the default size of the image -->
<%= image_tag @post.image.url(:medium) %> <!--Specifies the medium size of the image -->
<%= image_tag @post.image.url(:thumb) %> <!--Specifies the thumbnail size of the image -->
```




#### Understanding the Asset Pipeline and the Manifest file.

The Asset Pipeline basically does 4 things: 

* Concatenates CSS and Javascript files
* Compresses/Minifies CSS and Javascript files
* Precompiles the CSS and Javascript files
* Adds Assets fingerprinting



In order to require javascript or css files We need to configure it's manifest file

A manifest file is managed by our sprockets plugin in rails, A manifest file contains directives including asset files, Meaning a main file that requires all of the other dependency assets that we need to include in our application. Within that manifest file the asset pipeline does the 4 steps before it get served through our web server.



An Example might be, where a developer might want to require a custom css file from within the app/assets/stylesheets folder. he can do that by specifying the requirements inside of the application.css manifest file. 

His directory might look like this:

```tex
---app 
	---assets
		---stylesheets
			--- application.css(manifest file)
			--- main.css
			--- admin.css
			--- about.css
			--- home.css
		---javascripts
			---application.js(manifest file)
			--- admin.js
			--- about.js
			--- home.js
		---images
			--- someimage.png
```

##### application.css (simple way)

If we want to specify it's load order we can do it here in our manifest file

```css
/*
*=require_self (requires any css that resides in this file)
*=require admin (loads admin.css file and places it first to load before about)
*=require about (loads second to admin.css)
*=require_tree .(requires every css file in the directory)
*/
```

##### OR (ordered way)

A helpful way is assuming we have a lot of controllers and those controllers has a lot of actions and views and each view has a corresponding asset that ties to the view. Although it is really not recommended to have 90 +views, But to simplify.

Assuming we have a Controller that has 3 views which are index new and create and uses 3 separate assets, We can split up the code if were working with scss files.

If we structure it properly it would look like this.

```
---app 
	---assets
		---stylesheets
			--- application.css(manifest file)
			--- admin-folder
				--- admin.scss (import into main.scss)
				--- about.scss (import into main.scss)
				--- home.scss (import into main.scss)
				--- main.scss (the main file that gets compiled)
```

We stated that main.scss will be the main file that gets compile since, In Sass or Scss we can break up our code into smaller different pieces and define a main file that references to those files.

##### application.css (manifest file)

We can simply require the main file from the admin-folder like so

```css
/*
*=require ./admin/main
*/
```

#### Some Gotcha's to Note

We know that there are two ways to precompile our assets 

1. **Specifying them in our manifest file.** - This option is what should be used most of the time, and allows your Javascript and CSS files to be compiled into one application.js and one application.css file. The manifest is written at the top of the applicable asset.
2. **And Explicit precompile instruction in our initializers/assets.rb file** - This should rarely be done. Most of the time, the only reason you’d want to do this is if the asset is included alone on a special page where you don’t want the rest of the assets.

```ruby
# Specify the file name and it's file type 
Rails.application.config.assets.precompile += %w( filename.js )
```






### CSS Layouts in CSS 

You must have these comment line above your css stylesheet, with the corresponding information

```css
/* 
  Theme Name: 
  Author: 
  Author URI:
  Version: 1.0
*/
```

### Loading style resources 

```php
// this code lives in function.php
<?php 
  // Loads our css stylesheet 
  function resources() {
    wp_enqueue_style('style', get_stylesheet_uri());
  }
  add_action('wp_enqueue_scripts', 'resources');
?> 
```

### The Loop

"The Loop" - Loops through all of the post pages from the wordpress backend then displays them in the browser.

* ``if(have_posts())`` — checks and see if there any posts to display
* ``while(have_posts())`` — while loop conditional until every post gets displayed.
* ``the_permalink`` — shows the link url of the post
* ``the_title();``— shows the title of that relative post
* ``the_content();`` — shows the content/description of that relative post

```php
<?php 
  // The Loop loops posts inside wordpress
  if(have_posts()) :
    while(have_posts()) : the_post();
?> 
<article class="post">
  <h2> 
    <a href="<?php the_permalink(); ?>">
      <?php the_title(); ?>
    </a>
  </h2>
</article>


  <?php the_content();?>
<?php           
    endwhile;
  else: 
    echo "<p> No Content Found</p>";
  endif;
?>    
```

### Using Conditional Logic - if(is_page())

One way to 

* ``if(is_page("name-of-page-section")); or if(is_page("slug or ID"))``


### Specifying Specific Navigation menu links for nav menus

In Wordpress Under Appearance > Menus, You can specify which links go to which nav menu as when you create a new nav menu.

```php
// Primary Navigation / Top Navigation
<!--Primary Menu Navigation links-->
<?php 
  $args = array(
    'theme_location' => 'primary'
  );
?>
<?php wp_nav_menu($args); ?> // displays navigation links

// Footer Navigation / Bottom Navigation 
<?php 
  $args = array(
  	'theme_location' => 'footer'
  );
?>  
<?php wp_nav_menu($args); ?> //displays navigation links
// Navigation Menus 
register_nav_menus(array(
  'primary' => __('Primary Menu'),
  'footer' => __('Footer Menu')
));
```



### get_template_directory() vs get_template_directory_uri()

**get_template_directory** - echo's out the relative path when called.

**get_template_directory_uri()** - needs to explicitly state which file to choose, does not return a trailing slash following the directory address


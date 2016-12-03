# Setting up Simpleform to render errors via a ajax remote

```
gem 'simple_form'
```

Minimal form helper without label hints and so on. can be found here in this [link](https://github.com/plataformatec/simple_form/wiki/Creating-a-minimalist-form-(no-labels-just-placeholders))

Assuming we have a productsController 

which has a new(shows the form) and a create route for posting the data from the form.

##### ProductsController.rb

```ruby
def new
  @product = Product.new
end

def create
  @product = Product.new(product_params)
  if @product.save

    respond_to do |format|
      # format.html { redirect_to @product, notice: 'Product was successfully created.' }
      format.js
      # format.json { render :show, status: :created, location: @product }
    end

    else
      respond_to do |format|
      # format.html { render :new }
        format.js 
      # format.json { render json: @product.errors, status: :unprocessable_entity }
      end
    end
end

```



Styles to show the input which has error

```css
/* input fields*/
.has-error input{
  border: 1px solid #AA0000;
}
/* textarea fields*/
.has-error textarea {
  border: 1px solid #AA0000;
}
```



from inside the `app/helpers` create a file called `minimal_form_helper.rb`

Inside `minimal_form_helper.rb` 

```ruby
require "minimal_form_builder"
module MinimalFormHelper
  def minimal_form_for(object, *args, &block)
    options = args.extract_options!
    simple_form_for(object, *(args << options.merge(:builder => MinimalFormBuilder)), &block)
  end

  def minimal_fields_for(*args, &block)
    options = args.extract_options!
    simple_fields_for(*(args << options.merge(:builder => MinimalFormBuilder)), &block)
  end
end
```

then navigate through your `lib/` folder then create another file that extends through SimpleForm

name it as `lib/minimal_form_builder.rb`

```ruby
class MinimalFormBuilder < SimpleForm::FormBuilder
  def input(attribute_name, options = {}, &block)
    type = options.fetch(:as, nil) || default_input_type(
        attribute_name,
        find_attribute_column(attribute_name),
        options
    )

    if type != :boolean
      if options[:placeholder].nil?
        options[:placeholder] ||= if object.class.respond_to?(:human_attribute_name)
                                    object.class.human_attribute_name(attribute_name.to_s)
                                  else
                                    attribute_name.to_s.humanize
                                  end
      end
      options[:label] = false if options[:label].nil?
    end

    super
  end
end
```



Then instead of calling the simple_form_for helper we can call in our custom helper that we made earlier to remove the labels and non-neccessities to make a minimal form instead by simple calling it minimal_form_for.

In our `app/views/products/_form.html.erb` partial

This will remove all of the labels by defaut

```erb
<%= minimal_form_for @product, remote: true  do |f| %>
  <%= f.error_notification %>

  <div class="form-inputs">
    <%= f.input :name ,error: 'Username is mandatory, please specify one' %>
    <%= f.input :price, error: "Price is Mandatory, please specify one" %>
    <%= f.input :description, error: "Message is mandatory, please specify one" %>
  </div>

  <div class="form-actions">
    <%= f.button :submit %>
  </div>
<% end %>
```

Then the actual submission of data from this form is handle by our post route.

Since We specified earlier that the create route should respond to javascript. We simply need to make a create.js.erb file instead of the rails/html way ("create.html.erb"). 

In our `app/views/products/create.js.erb`

```erb
// if @contact.deliver && contact.errors.count == 0 send a success message
<% if @product && @product.errors.count == 0%>
// then remove the form on submit since this form is rendered in erb remove the second form that gets created when user clicks on submit possible bug fix in the future
  $('#new_product').remove();
  // If on success render the form again without refreshing
  // on success re render the form but instead it shows form partial as an update route
  $('.container').after('<%= j render("form") %>');
  // show the success message
  // Or render a success partial
  $("#notice").html("Success");

  // Clear the form if it's one route it will render the same route where the form resides
  $("form#new_product").find("input:text, input:number, textarea").val("");

<% else %>
  // If on error render the form with the errors without refreshing
  $('#new_product').remove();
  $('.container').after('<%= j render("form") %>');
  // Same as above remove the form so it doesn't re
  $('#notice').html("<%= j(render 'errors', :object => @product) %>");
<% end %>
```

Now everytime a user submits something correctly depending on our validations. the data will be sent remotely via ajax and be handled by our create route and show whether the post was saved or show errors without refreshing the page.
Index.html.erb

```erb	

<div class="container">
  <div class="row" style="margin-top: 50px;">
    <div class="col-lg-4 col-lg-offset-1">
      <%= link_to "Upload New Project", new_media_content_path, class: "btn btn-primary"%>
    </div>
  </div>
  <div class="row" style="margin-top: 25px;">
    <div class="col-lg-10 col-lg-offset-1">
      <div class="well">
        <h4><b>Media</b></h4><hr>
        <div class="row">
          <div class="col-lg-10">
            <%= form_tag '/delete_media', method: :delete do %>
              <%= submit_tag 'Delete', id: 'delete', class: 'btn btn-danger', disabled: @media_contents.empty? %>
              <br><br>
              <div class="row">
                <div id="media-contents" class="col-lg-12">
                  <% if @media_contents.empty? %>
                    <h5 id="no-media">No Media Found</h5>
                  <% else %>
                    <% @media_contents.each do |media| %>
                      <div class="col-lg-4">
                        <div class="thumbnail">
                          <!-- <%= image_tag media.file_name.url %> -->
                          <%=link_to(image_tag(media.file_name.url(:thumb)), :action => 'show', :id => media.id ) %>

                          <div class="caption">
                            <p>
                              <%= check_box_tag 'media_contents[]', media.id %>
                              <!-- <%= link_to('Show', {:action => 'show', :id => media.id})%> -->
                            </p>
                          </div>
                        </div>
                      </div>
                    <% end %>
                  <% end %>
                </div>
              </div>
            <% end %>
          </div>
          <div class="col-lg-1">
            <%= link_to 'Delete All', delete_all_path, method: :delete, id: 'delete-all', class: 'btn btn-danger', disabled: @media_contents.empty? %>
          </div>
        </div>
      </div>
    </div>
  </div>
</div>

```

_form.html.erb



```erb
<%= form_for(post) do |f| %>
  <% if post.errors.any? %>
    <div id="error_explanation">
      <h2><%= pluralize(post.errors.count, "error") %> prohibited this post from being saved:</h2>

      <ul>
      <% post.errors.full_messages.each do |message| %>
        <li><%= message %></li>
      <% end %>
      </ul>
    </div>
  <% end %>

  <div class="field">
    <%= f.label :name %>
    <%= f.text_field :name %>
  </div>

  <div class="field">
    <%= f.label :thumbnail %>
    <%= f.file_field :thumbnail %>
  </div>

  <div class="field">
    <%= f.label :info %>
    <%= f.text_field :info %>
  </div>

  <div class="actions">
    <%= f.submit %>
  </div>

  <%= f.fields_for @post.attachments,:url => {:action => "create"}, :html => {class: 'dropzone form', id: 'media-dropzone'} do |f| %>
    <div class="fallback">
      <%= f.file_field 'media', multiple: true %>
    </div>
  <% end %>
<% end %>

```


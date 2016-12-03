Dropzone Defaults, Trials and Errors 

`_form.html.erb`

```erb


<!-- <input type="file" multiple="multiple" name="upload[image][]" id="upload_image"> -->





<!-- <%= form_tag '/attachments', method: :post, class: 'dropzone form', id: 'media-dropzone' do %>
  <div class="fallback">
    <%= file_field_tag 'media', multiple: true %>
  </div>
<% end %> -->

<!-- Defaults -->
<!-- <div class="container">
  <div class="header">
    <h3>File Upload with Rails Dropzone and Carrierwave</h3>
    <%= form_for(@upload, :html => {multipart: true, class: "dropzone", id: "new_upload" }) do |f| %>
      <div class="dropzone-previews">
      </div>
      <%= f.file_field :image, type: :file, multiple: true %> <br>
      <%= f.submit "Upload" %>
    <% end %>
  </div>
</div> -->

```

`uploads.js`

```javascript
// // // Default from tutorial
// $(document).ready(function(){
// 	// disable auto discover
// 	Dropzone.autoDiscover = false;
// 	// grap our upload form by its id
// 	$("#new_upload").dropzone({
// 		// restrict image size to a maximum 1MB
// 		maxFilesize: 1,
// 		// changed the passed param to one accepted by
// 		// our rails app
// 		paramName: "upload[image]",
// 		// show remove links on each image upload
// 		addRemoveLinks: true,
// 		// if the upload was successful
// 		success: function(file, response){
// 			// find the remove button link of the uploaded file and give it an id
// 			// based of the fileID response from the server
// 			$(file.previewTemplate).find('.dz-remove').attr('id', response.fileID);
// 			// add the dz-success class (the green tick sign)
// 			$(file.previewElement).addClass("dz-success");
// 		},
// 		//when the remove button is clicked
// 		removedfile: function(file){
// 			// grap the id of the uploaded file we set earlier
// 			var id = $(file.previewTemplate).find('.dz-remove').attr('id');
//
// 			// make a DELETE ajax request to delete the file
// 			$.ajax({
// 				type: 'DELETE',
// 				url: '/uploads/' + id,
// 				success: function(data){
// 					console.log(data.message);
// 				}
// 			});
// 		}
// 	});
// });

// Alternative Code 1 works if autoProcessQueue is false
// And if the upload is not being submitted by a button click
// $(document).ready(function(){
// 	// disable auto discover
// 	Dropzone.autoDiscover = false;
// 	// grap our upload form by its id
// 	$("#new_upload").dropzone({
// 		maxFilesize: 1,
//     url: '/uploads',
// 		paramName: "upload[image]",
//     autoProcessQueue: false,
// 		addRemoveLinks: true,
//     uploadMultiple: true,
//
// 		// if the upload was successful
//     init: function() {
//       var myDropzone = this;
//       var submitButton = document.querySelector("input#upload")
//           myDropzone = this; // closure
//       submitButton.addEventListener("click", function() {
//         myDropzone.processQueue(); // Tell Dropzone to process all queued files.
//       });
//
//       $(this).on("success", function(file, response) {
//         $(file.previewTemplate).find('.dz-remove').attr('id', response.fileID);
//         // add the dz-success class (the green tick sign)
//         $(file.previewElement).addClass("dz-success");
//           window.location.href = ("/");
//       });
//     }
// 	});
// });






  // Alternative 2 The same as Alternative 1 Still doesn't work if on button submit via multiple file upload
  // // basic code for simple file upload on button click
  // var newUpload = $("#new_upload");
  // Dropzone.options.newUpload = {
  //   // Prevents Dropzone from uploading dropped files immediately
  //   autoDiscover: false,
  //   paramName: "upload[image]",
  //   autoProcessQueue: false,
  //   // maxFilesize: 2,
  //   // maxFiles: 3,
  //   uploadMultiple: true,
  //   addRemoveLinks: true,
  //   acceptedFiles: ".jpeg,.jpg,.png,.gif",
  //   parallelUploads: 100,
  //
  //   init: function() {
  //     var submitButton = document.querySelector("input[type=submit]")
  //         myDropzone = this; // closure
  //
  //     submitButton.addEventListener("click", function(e) {
  //       e.preventDefault();
  //       e.stopPropagation();
  //       myDropzone.processQueue(); // Tell Dropzone to process all queued files.
  //     });
  //     // this.on("processingmultiple", function(file) {
  //     //   alert("Added File");
  //     // });
  //     this.on("success", function(file, responseText) {
  //       window.location.href = ("/")
  //     });
  //     this.on("maxfilesexceeded", function(file){
  //         this.removeFile(file);
  //         alert("No more files please!");
  //     });
  //   }
  // };
//
```




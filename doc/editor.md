# 编辑器

<div id="ex-editor-01">
  <tinymce ref="editor" :name="name" v-model="value" :options="options"></tinymce>
</div>
<script>
var ex_editor_01 = new Vue({
  el: '#ex-editor-01',
  data: function () {
    return {
      name: 'editor',
      value: 'Hello',
      options: {
        images_upload_url: 'postAcceptor.php',
        image_title: true, 
        // file_picker_callback: function(cb, value, meta) {
        //   var input = document.createElement('input');
        //   input.setAttribute('type', 'file');
        //   input.setAttribute('accept', 'image/*');
        //   input.onchange = function() {
        //     var file = this.files[0];
        //     var reader = new FileReader();
        //     reader.onload = function () {
        //       // Note: Now we need to register the blob in TinyMCEs image blob
        //       // registry. In the next release this part hopefully won't be
        //       // necessary, as we are looking to handle it internally.
        //       var id = 'blobid' + (new Date()).getTime();
        //       var blobCache =  tinymce.activeEditor.editorUpload.blobCache;
        //       var base64 = reader.result.split(',')[1];
        //       var blobInfo = blobCache.create(id, file, base64);
        //       blobCache.add(blobInfo);
        //       // call the callback and populate the Title field with the file name
        //       cb(blobInfo.blobUri(), { title: file.name });
        //     };
        //     reader.readAsDataURL(file);
        //   };
        //   input.click();
        // },
        // images_upload_handler: function (blobInfo, success, failure) {
        //   setTimeout(function() {
        //     // no matter what you upload, we will turn it into TinyMCE logo :)
        //     success('http://moxiecode.cachefly.net/tinymce/v9/images/logo.png');
        //   }, 2000);
        // },
        file_picker_types: 'file image',
        paste_data_images: true
      }
    }
  }
})
</script>
# 文件上传

<div id="ex-uploader-01">
  <uploader-file
  post-action="http://localhost:8000/upload"
  @input="handleFiles"
  :on-success="onSuccess"
  :on-start="onStart"
  :on-upload="onUpload"
  :on-delete="onDelete"
  :on-error="onError"
  :on-progress="onProgress"
  multiple
  >上传文件</uploader-file>
  <ul>
    <li v-for="f in files">{{f.name}} ---> {{f.active ? f.progress + '%' : f.status}}</li>
  </ul>
</div>

<script>
var ex_uploader_01 = new Vue({
  el: '#ex-uploader-01',
  data: {
    files: [],
  },
  methods: {
    handleFiles: function (v){
      this.files = v
    },
    onStart: function(newFile){
      console.log('start', newFile)
    },
    onSuccess: function (success, newFile) {
      console.log(newFile.response)
      console.log('success', success, newFile)
    },
    onError: function (error, newFile) {
      console.log('error', error, newFile)
    },
    onDelete: function (newFile) {
      console.log('delete', newFile)
    },
    onUpload: function (newFile) {
      console.log('upload', newFile)
    },
    onProgress: function(progress, newFile) {
      console.log('progress', progress, newFile)
    }
  }
})
</script>

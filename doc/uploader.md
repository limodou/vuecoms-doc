# 文件上传

<div id="ex-uploader-01">
  <uploader-file
  ref='uploader'
  post-action="http://localhost:8000/upload"
  @input="handleFiles"
  :on-success="onSuccess"
  :on-start="onStart"
  :on-upload="onUpload"
  :on-delete="onDelete"
  :on-error="onError"
  :on-progress="onProgress"
  >上传文件</uploader-file>
  <ul>
    <li v-for="f in files">{{f.name}} ---> {{f.active ? f.progress + '%' : f.status}}</li>
  </ul>
      <uploader-file
  ref='uploader1'
  post-action="http://localhost:8000/upload"
  @input="handleFiles1"
  :on-success="onSuccess"
  :on-start="onStart"
  :on-upload="onUpload"
  :on-delete="onDelete"
  :on-error="onError"
  :on-progress="onProgress"
  :on-before-open="onBeforeOpen"
  :manual="true"
  >手动上传文件</uploader-file>
    <ul>
    <li v-for="f in files1">{{f.name}} ---> {{f.active ? f.progress + '%' : f.status}}</li>
  </ul>
  <i-button @click="handleClick">HTML5直接上传</i-button><br/><br/>
</div>

<script>
var ex_uploader_01 = new Vue({
  el: '#ex-uploader-01',
  data: {
    files: [],
    files1: []
  },
  methods: {
    handleFiles: function (v){
      this.files = v
    },
    handleFiles1: function (v){
      this.files1 = v
    },
    onStart: function(newFile){
      console.log('start', newFile)
      console.log('file', this.$refs.uploader.$refs.upload.$refs.file.$el)
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
    },
    handleClick: function () {
      var base64 = 'data:image/jpg;base64,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'
      this.$fileupload({name: 'filename', content: base64, filename: 'a.jpg'}, {action: 'http://localhost:8000/upload', onprogress: function(percent) {
        console.log(percent)
      }}).then(function(r){
        console.log('success', r)
      })
    },
    onBeforeOpen: function (el) {
      alert('上传前')
      el.open()
    }
  }
})
</script>

文件上传组件目前可以支持单个文件上传，暂不支持多文件上传
## 参数说明

| 属性参数名 | 说明 | 类型 | 缺省值 |
|----------|-----|------|------|
| postAction | 提交到后台的地址 | String | 必填 |
| manual | 是否手工触发打开文件浏览窗口 | Boolean | False |
| onSuccess | 成功处理事件回调 onSuccess(success, newFile)，success 为成功状态，类型为 Boolean，成功为 true。newFile 是上后成功后的文件对象 |  Function | null |
| onStart | 开始上传事件回调 onStart(newFile)，newFile 是将要上传的文件对象 |  Function | null |
| onUpload | 上传事件回调 onUpload(newFile)，newFile 是将要上传的文件对象 |  Function | null |
| onUpload | 成功处理事件回调 onUpload(newFile)，newFile 是将要删除的文件对象 |  Function | null |
| onError | 出错处理事件回调 onError(error, newFile)，error 为出错状态，类型为 Boolean，出错为 true。newFile 是正在处理的文件对象 |  Function | null |
| onProgress | 上传进度处理事件回调 onProgress(progress, newFile)， progress 为处理进度，类型为 Number。newFile 是正在处理的文件对象 |  Function | null |
| onBeforeOpen | 打开文件前事件回调 onBeforeOpen(el)，el 是上传组件对象，可以调用它的 open() 方法，手动打开文件浏览窗口进行文件上传 |  Function | null |


## 方法说明

| 方法名 | 说明 | 返回值 |
|----------|-----|------|
| open | 手动打开文件浏览窗口 | 无 |

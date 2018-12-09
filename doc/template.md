# 动态模板

<div id="app">
  <i-button @click="handlerClick">更新</i-button>
  <u-template :template="t"></u-template>
</div>

<script>
Vue.component('tt', {
  props: ['template'],
  render: function(h) {
    var d = {template: this.template}
    return h(d)
  }
})
var app = new Vue({
  el: '#app',
  data: {
    t: [
        '<Tabs value="name1">',
          '<TabPane label="标签一" name="name1">标签一的内容</TabPane>',
          '<TabPane label="标签二" name="name2">标签二的内容</TabPane>',
          '<TabPane label="标签三" name="name3">标签三的内容</TabPane>',
        '</Tabs>'
      ].join('')
  },
  methods: {
    handlerClick: function () {
      var self = this
      self.t = [
        '<Box title="这是标题">',
          '<p>这是内容</p>',
          '<p>这是内容</p>',
          '<p>这是内容</p>',
          '<p>这是内容</p>',
        '</Box>'
      ].join('')
    }
  }
})
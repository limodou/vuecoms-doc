# Toast 组件

因为在 iview 中没有提供 Toast 组件，所以 vuecoms 提供了一个。

<div id="ex-toast-01">
  <i-button type="primary" @click="handleToast">显示Toast</i-button>
</div>

<script>
var ex_toast_01 = new Vue({
  el: '#ex-toast-01',
  methods: {
    handleToast: function () {
      var self = this
      this.$toast({content: 'Hello', type: 'success', showIcon: true, delay: 5000,
        onClose: function() {
          self.$Message.info('关闭执行')
        }
      })
    }
  }
})
</script>


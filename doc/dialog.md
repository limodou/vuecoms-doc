# 对话框

对话框的处理可以直接使用 iview 的 Modal 组件，但是不方便的地方就是需要在使用它的组件某个地方定义。

Vuecoms 提供了通用的对话框组件以及可以通过绑定到 Vue.prototype.$Dialog 和 Vue.prototype.$ConfirmDialog 来直接调用，简化了对话框的定义。

## 示例一，$Dialog

<div id="ex-dialog-01">
  <i-button type="primary" @click="handleClick">Show Modal</i-button>
</div>
<script>
var component = Vue.component('dialog', {
  template: '<build ref="build" :data="data" :value="value"></build>',
  props: ['value'],
  data: function () {
    var data = [
      {
        name: 'basic1',
        title: '基本信息1',
        boxComponent: '',
        static: true,
        fields: [
          {name: 'str1', label: '字符串1', placeholder: '请输入...', help: '帮助信息',
            info: 'info信息', required: true, rule: {type: 'email'}},
          {name: 'str2', label: '静态字符串2', static: true, required: true, convert: function(v){
            return '<a href="#">' + v + '</a>'
            }
          },
        ],
      }
    ]
    return {
      data: data,
    }
  }
})
var ex_dialog_01 = new Vue({
  el: '#ex-dialog-01',
  methods: {
    handleClick: function () {
      this.$Dialog({
        title: '对话框标题',
        component: component,
        props: {
          value: {str1: 'abc@gmail.com', str2: 'Demo'}
        },
        buttons: [
          [
            {
              label: '关闭',
              type: 'default',
              onClick(target) {
                target.close();
              },
            },
          ],
        ],
      })
    }
  }
})
</script>

演示了：

1. 定义一个组件，通过 `this.$Dialog` 直接在对话框加打开，不需要定义 Modal 了
2. 使用 $Dialog 时，可以自定义按钮。

Dialog 属性 props (和 Modal 基本类似)

| 属性 | 说明 | 类型 | 缺省值 |
|-----|------|-----|-------|
| component | 底层组件，可以是组件名称（需要全局注册）也可以是组件对象或异步组件。 |	String|Object|Function |  |
| title | 对话框标题 | String | '' |
| props | 传递给底层组件的属性 | Object | |
| on | 将要捕获底层组件的事件及处理。key 是事件名，value 是处理函数 | Object | |
| name | 底层组件的 ref 的名字 | String | |
| onOk | 点击确定时调用的回调函数。当点击时确定按钮会显示 loading 状态，如果 onOk 定义了，则当它返回 true 时，会自动关闭对话框。如果返回 false，则对话框仍然显示，loading 状态取消。如果未定义 onOk，则直接关闭对话框. onOk 可以是 Promise 对象。 | Function|Promise | |
| onCancel | 当点击取消时的回调函数，可以是 Promise 对象。 | Function|Promise | |
| buttons | 自定义按钮。同 Grid, Build 的按钮定义形式相同。 | Array | |
| buttonSize | 按钮大小。可以是 large, small, default | String | 'default' |
| width | 对话框宽度，用 px 表示 | String | |
| closable | 是否显示右上角关闭按钮 | Boolean | true |
| maskClosable | 是否点击遮罩时关闭对话框 | Boolean | true |
| draggable | 是否可以拖动对话框 | Boolean | false |

Dialog 方法

| 名称 | 说明 |
|-----|------|
| close | 关闭当前对话框 |

## 示例二，$ConfirmDialog

它的作用是显示一个用于输入的对话框，因此要传入 value 属性，同时要执行底层组件的 validate 和 getData 方法，来校验和获取数据。然后将得到的数据传给 onOk 回调函数。如果 onOk 处理成功，应返回 true。validate 和 getData 都可以省略。

<div id="ex-dialog-02">
  <i-button type="primary" @click="handleClick">Show Modal</i-button>
</div>
<script>
var component = Vue.component('dialog', {
  template: '<build ref="build" :data="data" :value="value"></build>',
  props: ['value'],
  data: function () {
    var data = [
      {
        name: 'basic1',
        title: '基本信息1',
        boxComponent: '',
        fields: [
          {name: 'str1', label: '字符串1', placeholder: '请输入...', help: '帮助信息',
            info: 'info信息', required: true, rule: {type: 'email'}},
          {name: 'str2', label: '静态字符串2', static: true, required: true, convert: function(v){
            return '<a href="#">' + v + '</a>'
            }
          },
        ],
      }
    ]
    return {
      data: data,
    }
  },
  methods: {
    validate () {
      return this.$refs.build.validate()
    },
    getData() {
      return this.$refs.build.getData();
    },
  }
})
var ex_dialog_02 = new Vue({
  el: '#ex-dialog-02',
  methods: {
    handleClick: function () {
      this.$ConfirmDialog({
        title: '对话框标题',
        component: component,
        props: {
          value: {str1: 'abc@gmail.com', str2: 'Demo'}
        },
        onOk (data) {
          console.log(data)
          return true
        }
      })
    }
  }
})
</script>

ConfirmDialog 属性 props (和 Dialog 基本类似)

| 属性 | 说明 | 类型 | 缺省值 |
|-----|------|-----|-------|
| component | 底层组件，可以是组件名称（需要全局注册）也可以是组件对象或异步组件。 |	String|Object|Function |  |
| title | 对话框标题 | String | '' |
| props | 传递给底层组件的属性 | Object | |
| on | 将要捕获底层组件的事件及处理。key 是事件名，value 是处理函数 | Object | |
| value | 传入给底层组件的 value 属性 | Any | |
| name | 底层组件的 ref 的名字 | String | |
| onOk | 点击确定时调用的回调函数。当点击时确定按钮会显示 loading 状态，如果 onOk 定义了，则当它返回 true 时，会自动关闭对话框。如果返回 false，则对话框仍然显示，loading 状态取消。如果未定义 onOk，则直接关闭对话框. onOk 可以是 Promise 对象。 | Function|Promise | |
| onCancel | 当点击取消时的回调函数，可以是 Promise 对象。 | Function|Promise | |
| buttons | 自定义按钮。同 Grid, Build 的按钮定义形式相同。 | Array | |
| buttonSize | 按钮大小。可以是 large, small, default | String | 'default' |
| width | 对话框宽度，用 px 表示 | String | |
| closable | 是否显示右上角关闭按钮 | Boolean | true |
| maskClosable | 是否点击遮罩时关闭对话框 | Boolean | true |
| draggable | 是否可以拖动对话框 | Boolean | false |


Dialog 方法

| 名称 | 说明 |
|-----|------|
| close | 关闭当前对话框 |

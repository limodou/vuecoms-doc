# 自定义Box

本例演示了在Build中替换原有的Box，增加自定义的内容。是在Box基础上进行定制。主要包括以下几点：

1. 使用 `theme="card"` 的风格，和iview兼容
2. 不显示收起按钮
3. 定制title效果，加icon和点击链接
4. 底部按钮居中显示

<div id="ex-build-01">
  <build ref="build" :data="data" :value="value" :choices="choices" :errors="errors" ></build>
  <div>{{value}}</div>
</div>
<script>
Vue.component('mybox', {
  template: ['<Box theme="card" style="margin-bottom:20px" :removerable="false" :collapse="false" v-bind="$options.propsData">',
  '<p slot="title"><Icon :type="icon"></Icon> {{title}} <a @click.prevent="handleClick" style="font-size: 12px">点击示例</a></p>',
  '<i-button slot="tools" size="small">新增</i-button>',
  '<div slot="footer"><slot name="footer"></slot></div>',
  '<slot></slot>',
  '</Box>'].join(''),
  props: ['title', 'icon', 'withBorder', 'withFooterBorder', 'footerAlign', 'headerAlign'],
  methods: {
    handleClick: function() {
      console.log(this)
      this.$Message.info("点击示例")
    }
  }
})
var ex_build_01 = new Vue({
  el: '#ex-build-01',
  data: function () {
    var self = this
    var data = [
      {
        name: 'basic',
        title: '基本信息',
        labelWidth: 150,
        staticSuffix: '_static',
        fields: [
          {name: 'select1', label: '选择1', type: 'select', required: true, options: {clearable: true,
              // choices: [
              //   {label:'选项一', value: 'A'},
              //   {label:'选项二', value: 'B'},
              // ] 
            },
            onChange: function (value, alldata) {
              self.$set(alldata, 'select2', value)
            }
          },
          {name: 'select2', label: '选择2', type: 'str', static: true},
          {name: 'select3', label: '选择3', type: 'select', required: true, multiple: true, options: {clearable: true,
              // choices: [
              //   {label:'选项一', value: 'A'},
              //   {label:'选项二', value: 'B'},
              // ] 
            }
          },
        ],
        layout: [
          ['select1', 'select2'],
          [{name: 'select3', colspan: 12}]
        ],
        boxComponent: 'mybox',
        boxOptions: {icon: 'ios-albums-outline', withBorder: true, withFooterBorder: false, footerAlign: 'center'},
        buttons: {
          items: [
            [{label: '查看结果', type:'primary', onClick: function(target, data){
                console.log(target, data)
              }
            }],
          ],
        }
      },
    ]
    return {
            data:data,
            value: {
              select1: '',
              select2: ''
            },
            choices: {
              select1: [],
              select3: []
            },
            errors: {},
          }
  },
  methods: {
    save: function(error) {
      if (error) {
        this.$Message.error(error)
      } else {
        this.$Message.info('saved')
      }
    }
  },
  mounted: function () {
    var self = this
    setTimeout(function () {
      self.$set(self.value, 'select1', 'A')
      self.$set(self.value, 'select3', ['A', 'B'])
    }, 50)
    setTimeout(function () {
      var c = [
        {label:'选项一', value: 'A'},
        {label:'选项二', value: 'B'},
        {label:'选项三', value: 'C'}
      ]
      self.$set(self.choices, 'select1', c)
      self.$set(self.choices, 'select3', c)
    }, 1000)
  }
})
</script>

----

上述Box的示例代码：

```
Vue.component('mybox', {
  template: ['<Box theme="card" style="margin-bottom:20px" :removerable="false" :collapse="false" v-bind="$options.propsData">',
  '<p slot="title" class="box-title"><Icon :type="icon"></Icon> {{title}} <a @click.prevent="handleClick" style="font-size: 12px">点击示例</a></p>',
  '<i-button slot="tools" size="small">新增</i-button>',
  '<div slot="footer"><slot name="footer"></slot></div>',
  '<slot></slot>',
  '</Box>'].join(''),
  props: ['title', 'icon', 'withBorder', 'withFooterBorder', 'footerAlign', 'headerAlign'],
  methods: {
    handleClick: function() {
      console.log(this)
      this.$Message.info("点击示例")
    }
  }
})
```

本示例是直接在js中编写，未写成单文件组件方式，所以单文件方式可以自行修改。

* `v-bind="$options.propsData"` 是用于方便向Box传递外部设置的参数。同时，在 `props` 中定义了可以让外部使用的参数。
* `<p slot="title"><Icon :type="icon"></Icon> {{title}} <a @click.prevent="handleClick" style="font-size: 12px">点击示例</a></p>` 实现了自定义标题头。要注意指明 `slot="title"`。
* `<i-button slot="tools" size="small">新增</i-button>` 实现了自定义的右上角工具按钮。
* `<div slot="footer"><slot name="footer"></slot></div>` 实现了 footer 处理，以便向原 Box 的 footer 进行内容传递。

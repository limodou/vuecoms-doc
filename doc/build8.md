# 观查者模式

在处理Build时经常会遇到的一个问题就是动态改变字段的hidden, static, required，那么一般的做法是通过 build.fields[name].[hidden|static|required] 来修改它的值，但是这种不是很方便，因此 Build 提供了一种根据状态对象来自动跟踪变化的机制。

首先你需要定义一个对象，如：

```
statusObject = {
  status1: function () {
    return 表达式1
  },
  status2: function () {
    return 表达式2
  },
  status3: function () {
    return 表达式3
  }
}
```

然后传入build对象，如： `:status-object="statusObject"`

在定义fields时，分别使用:

```
{name: 'str11', label: '字段11', help: '当选择1勾选时可见', showWhen: 'status1'}'
{name: 'str2', label: '字段2', help: '当选择2勾选时静态', staticWhen: 'status2'},
{name: 'str3', label: '字段3', help: '当选择3勾选时必填', requiredWhen: 'status3'},
```

来关联字段与状态的关系。


<div id="ex-build-01">
  <build ref="build" :data="data" :value="value" :status-object="statusObject"></build>
  <div>{{value}}</div>
</div>
<script>
var ex_build_01 = new Vue({
  el: '#ex-build-01',
  data: function () {
    var self = this
    var data = [
      {
        name: 'basic',
        title: '基本信息',
        fields: [
          {name: 'check1', label: '选择1', type: 'checkbox'},
          {name: 'check2', label: '选择2', type: 'checkbox'},
          {name: 'check3', label: '选择3', type: 'checkbox'},
          {name: 'str11', label: '字段11', help: '当选择1勾选时可见', showWhen: 'status1'},
          {name: 'str12', label: '字段12', help: '当选择1勾选时可见', showWhen: 'status1'},
          {name: 'str2', label: '字段2', help: '当选择2勾选时静态', staticWhen: 'status2'},
          {name: 'str3', label: '字段3', help: '当选择3勾选时必填', requiredWhen: 'status3'},
          {name: 'str4', label: '字段4', help: '当选择3勾选时处理', when: ['status3', function(v, field, value){
            self.$Message.info('字段4被调用:'+v)
          }]},
          {name: 'str5', label: '字段5', help: '当选择2和3勾选时处理', when: [function(){return self.value.check2 && self.value.check3}, function(v, field, value){
            self.$Message.info('字段5被调用:'+v)
          }]},
        ],
        layout: [
          ['check1'],
          ['check2'],
          ['check3'],
          ['str11', 'str12'],
          ['str2'],
          ['str3'],
          ['str4'],
          ['str5'],
        ]
      },
    ]
    return {
            data:data,
            value: {},
            statusObject: {
              status1: function(){
                return self.value.check1
              },
              status2: function(){
                return self.value.check2
              },
              status3: function(){
                return self.value.check3
              },
            }
          }
  },
  // mounted () {
  //   this.$watch(function(){return this.value.radio === '1'}, function(newVal, oldVal){
  //     console.log('=====', newVal)
  //   })
  // }
})
</script>

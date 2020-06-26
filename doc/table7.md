# 表格过滤

演示了如何在Grid 中使用列的过滤机制。它可以在定义 Column 时传入 `filterable` 属性，就象是 editor 类似，可以传入指定类型(type)的组件，如果有额外的组件参数可以在 `options` 中传入。如果提供了 `label`，则会显示在弹出层的上面。

例如常见的下拉选择，vuecoms 编写了一个叫 flat-select 的组件，可以在定义表头列时可以这样写：

``` js
{name:'name2', title:'Name2', width:200, filterable: {
  type: 'flat-select',
  options: {
    choices: [
      {value: '', label: '全部'},
      {value: '>B3', label: '大于 B3'},
      {value: '<=B3', label: '小于等于 B3'}
    ]
  }
}},
```

在展示时，会在表头列名称的右侧显示一个漏斗的图标，当显示为浅灰色时，表示未输入条件，当显示为深色填入图标时，表示用户输入了条件。

过滤器在用户输入时，会立即触发查询，并且会将对应的值传入 `onLoadData` 的 `param` 参数中。如上面例子，当选择了 "大于 B3"时，在 `param` 中会有 `{name2: '>B3'}` 的内容，因此用户可以根据这个来进行数据的过滤。

下面的示例，则演示了几种过滤组件的写法，以及如何实现前端分页及过滤。是通过 `onLoadData` 来处理的。

<div id="ex-table-07">
  <Grid ref="table" :data="table"></Grid>
</div>
<script>
var ex_table_07 = new Vue({
  el: '#ex-table-07',
  data: function () {
    var self = this
    var table = {
      nowrap: true,
      draggable: true,
      indexCol: true,
      checkCol: true,
      clickSelect: true,
      columns: [
        {name:'name1', title:'Name1', width:200},
        {name:'name2', title:'Name2', width:200, filterable: {
          type: 'flat-select',
          options: {
            choices: [
              {value: '', label: '全部'},
              {value: '>B3', label: '大于 B3'},
              {value: '<=B3', label: '小于等于 B3'}
            ]
          }
        }},
        {name:'name3', title:'Name3', width:200, filterable: {
          label: '输入名称',
          options: {
            clearable: true
          }
        }},
        {name:'name4', title:'Name4', width:200, filterable: {
          type: 'flat-select',
          options: {
            multiple: true,
            choices: [
              {value: '>D3', label: '大于 D3'},
              {value: '=D3', label: '等于 D3'},
              {value: '<D3', label: '小于 D3'},
            ]
          }
        }},
      ],
      onLoadData: function (url, param, callback) {
        console.log('===== param =====', param)
        let filters = []
        var f_name2 = function(item) {
          if (!param.name2) return true
          if (param.name2 === '>B3') return item.name2>'B3'
          return item.name2 <= 'B3'
        }
        filters.push(f_name2)
        var f_name3 = function(item) {
          if (!param.name3) return true
          return item.name3.indexOf(param.name3) > -1
        }
        filters.push(f_name3)
        var f_name4 = function(item) {
          if (!param.name4) return true
          var flag = false
          for(let i=0, len=param.name4.length; i<len; i++) {
            var c = param.name4[i]
            if (c === '>D3') flag = flag || item.name4>'D3'
            else if (c === '=D3') flag = flag || item.name4 === 'D3'
            else flag = flag || item.name4 < 'D3'
          }
          return flag
        }
        filters.push(f_name4)
        var d = self.data.filter(function(item){
          var flag = true
          for(var i=0, len=filters.length; i<len; i++) {
            flag = flag && filters[i](item)
          }
          return flag
        })
        callback(d, {total: d.length})
      }
    }
    var data = []
    data.push({id:1, name1:'A1', name2:'B1', name3:'C1', name4:'D1'})
    data.push({id:2, name1:'A2', name2:'B2', name3:'C2', name4:'D2'})
    data.push({id:3, name1:'A3', name2:'B3', name3:'C3', name4:'D3'})
    data.push({id:4, name1:'A4', name2:'B4', name3:'C4', name4:'D4'})
    data.push({id:5, name1:'A5', name2:'B5', name3:'C5', name4:'D5'})
    data.push({id:6, name1:'A6', name2:'B6', name3:'C6', name4:'D6'})
    data.push({id:7, name1:'A7', name2:'B7', name3:'C7', name4:'D7'})
    return {table:table, data: data}
  },
  methods: {
    handleDrag: function (v) {
      console.log(v)
    }
  }
})
</script>

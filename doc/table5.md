# 树型表格

<div id="ex-table-05">
  <Grid :data="table" ref="grid"></Grid>
</div>

<script>
var ex_table_05 = new Vue({
  el: '#ex-table-05',
  data: function () {
    var self = this
    var table = {
      nowrap: true,
      indexCol: true,
      theme: 'default',
      editMode: 'row',
      actionColumn: 'name6',
      // tree 参数
      tree: true,
      treeField: 'name4',
      columns: [
        {name:'name1', title:'Name1', width:100, fixed: 'left'},
        {name:'name2', title:'Name2', width: 100},
        {name:'name3', title:'Name3', width:100},
        {name:'name4', title:'Name4', align:'left', width:400, editor: {}},
        {name:'name5', title:'Name5', width:400, render: function(h, param) {
          var self = this
          return h('Checkbox', {
            props: {
              value: param.value
            },
            on: {
              input: function(value) {
                alert(self.value)
              }
            }
          })
        }},
        {name:'name6', title:'Name6', fixed: 'right', width: 120}
      ],
      data: [],
      buttons: [
        [
          {label: '切换样式', type: 'primary', onClick: function () {
            if (self.$refs.grid.theme === 'default')
              self.$refs.grid.theme = 'simple'
            else
              self.$refs.grid.theme = 'default'
          }}
        ]
      ],
      onLoadData: function(url, param, callback) {
        if (param.parent) {
          var d = []
          var id
          var isParent = false
          for (var i=1; i<5; i++) {
            id = param.parent+'-'+i
            if (i === 3) {
              isParent = true
            } else {
              isParent = false
            }
            d.push({id:id, check: 'A'+id, name1:'A'+id, name2:'B'+id, name3:'C'+id, name4:'D'+id, name5:'E'+id,  _isParent: isParent, parent: param.parent})
          }
          setTimeout(function () {
            callback(d)
          }, 1000)
        } else {
          callback()
        }
      }
    }
    table.data.push({id:1, check: 'A1', name1:'A1', name2:'B1', name3:'C1', name4:'D1', name5:'E1',  _expand: false, _isParent: true})
    table.data.push({id:2, check: 'A1', name1:'A1', name2:'B1', name3:'C1', name4:'D1', name5:'E1', name6:'F1', parent: 1, _level: 1, _isParent: true})
    table.data.push({id:3, check: 'A1', name1:'A1', name2:'B2', name3:'C1', name4:'D1', name5:'E1', name6:'F1', parent: 2, _level: 2})
    table.data.push({id:4, check: 'A2', name1:'A2', name2:'B2', name3:'C1', name4:'D1', name5:'E2', name6:'F1', parent: 1, _level: 1})
    table.data.push({id:5, check: 'A2', name1:'A2', name2:'B2', name3:'C1', name4:'D1', name5:'E2', name6:'F1', parent: 1, _isParent: true})
    table.data.push({id:6, check: 'A1', name1:'A1', name2:'B1', name3:'C1', name4:'D1', name5:'E3', name6:'F1'})

    return {table:table}
  }
})
</script>

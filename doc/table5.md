# 树型表格

<div id="ex-table-05">
  <Grid :data="table" ref="grid"></Grid>
</div>

<script>
var ex_table_05 = new Vue({
  el: '#ex-table-05',
  data: function () {
    var self = this
    var order = 1
    var table = {
      nowrap: true,
      indexCol: true,
      checkCol: true,
      multiSelect: true,
      theme: 'default',
      editMode: 'row',
      actionColumn: 'name6',
      idField: 'id',
      orderField: 'order',
      clickSelect: true,
      // tree 参数
      tree: true,
      treeField: 'name4',
      columns: [
        {name:'id', title:'ID', width:40},
        {name:'name1', title:'Name1', width:100, fixed: ''},
        {name:'name2', title:'Name2', width: 100},
        {name:'name3', title:'Name3', width:100},
        {name:'name4', title:'Name4', align:'left', editor: {}},
        {name:'name5', title:'Name5', width:120, render: function(h, param) {
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
        {name:'name6', title:'Name6', fixed: '', width: 120}
      ],
      data: [],
      buttons: [
        [
          {label: '切换样式', type: 'primary', onClick: function () {
            if (self.$refs.grid.theme === 'default')
              self.$refs.grid.theme = 'simple'
            else
              self.$refs.grid.theme = 'default'
          }},
          {label: '增加第一子结点', type: 'primary', onClick: function (target, store) {
            var rows = store.getSelectedRows()
            if (rows.length === 0) {
              self.$Message.info('请先选择')
            } else {
              store.addEditChildRow({id:8, check: 'A8', name1:'A8', name2:'B8', name3:'C8', name4:'D8', name5:'E8', name6:'F8'}, rows[0], 'before')
            }
          }},
          {label: '增加最后子结点', type: 'primary', onClick: function (target, store) {
            var rows = store.getSelectedRows()
            if (rows.length === 0) {
              self.$Message.info('请先选择')
            } else {
              store.addEditChildRow({id:8, check: 'A8', name1:'A8', name2:'B8', name3:'C8', name4:'D8', name5:'E8', name6:'F8'}, rows[0], 'after')
            }
          }},
          {label: '向后增加结点', type: 'primary', onClick: function (target, store) {
            var rows = store.getSelectedRows()
            if (rows.length === 0) {
              self.$Message.info('请先选择')
            } else {
              store.addEditRow({id:8, check: 'A8', name1:'A8', name2:'B8', name3:'C8', name4:'D8', name5:'E8', name6:'F8'}, rows[0], 'after')
            }
          }},
          {label: '向前增加结点', type: 'primary', onClick: function (target, store) {
            var rows = store.getSelectedRows()
            if (rows.length === 0) {
              self.$Message.info('请先选择')
            } else {
              store.addEditRow({id:9, check: 'A9', name1:'A9', name2:'B9', name3:'C9', name4:'D9', name5:'E9', name6:'F9'}, rows[0], 'before')
            }
          }}
        ],
        [
          {label: '设置选中', type: 'primary', onClick: function (target, store) {
            store.setSelection([1, 9])
            }
          },
          {label: '清除所有选中', type: 'primary', onClick: function (target, store){
            store.deselectAll()
          }}
        ],
        [
          {label: '置顶', type: 'primary', onClick: function (target, store) {
              var rows = store.getSelectedRows()
              if (rows.length === 0) {
                self.$Message.info('请先选择')
              } else {
                store.moveRow(rows[0], 'first') 
              }
            }
          },
          {label: '上移', type: 'primary', onClick: function (target, store) {
              var rows = store.getSelectedRows()
              if (rows.length === 0) {
                self.$Message.info('请先选择')
              } else {
                store.moveRow(rows[0], 'up')
              }
            }
          },
          {label: '下移', type: 'primary', onClick: function (target, store) {
              var rows = store.getSelectedRows()
              if (rows.length === 0) {
                self.$Message.info('请先选择')
              } else {
                store.moveRow(rows[0], 'down')
              }
            }
          },
          {label: '置底', type: 'primary', onClick: function (target, store) {
              var rows = store.getSelectedRows()
              if (rows.length === 0) {
                self.$Message.info('请先选择')
              } else {
                store.moveRow(rows[0], 'last')
              }
            }
          }
        ],
        [
          {label: '全部展开', name: 'expandAll', type: 'primary', onClick: function(grid, store) {
            grid.expand()
          }},
          {label: '全部收起', name: 'collapseAll', type: 'primary', onClick: function(grid, ststoreroe) {
            grid.collapse()
          }}
        ],
        [
          {label: '设置联动选择', name: 'select', type: 'primary', onClick: function (target, store) {
              store.states.checkStrictly = !store.states.checkStrictly
              // target.btns['select'].label = '取消联动选择'
            }
          }
        ]
      ],
      onLoadData: function(url, param, callback) {
        if (param.parent) {
          var d = []
          var id
          var isParent = false
          for (var i=1; i<5; i++) {
            id = param.parent+'-'+i
            d.push({id:id, check: 'A'+id, name1:'A'+id, name2:'B'+id, name3:'C'+id, name4:'D'+id, name5:'E'+id, order: order++}
            )
          }
          setTimeout(function () {
            callback(d)
          }, 0)
        } else {
          callback()
        }
      },
      onSaveRow: function (row, callback) {
        callback('ok', row)
      },
      onDeleteRow: function (row, callback) {
        callback('ok', row)
      },
      onMove: function(order, callback) {
        console.log('order', order)
        callback(true)
      }
    }
    table.data = [
      {id:1, check: 'A1', name1:'A1', name2:'B1', name3:'C1', name4:'D1', name5:'E1', order: order++, children: []},
      {id:6, check: 'A1', name1:'A1', name2:'B1', name3:'C1', name4:'D1', name5:'E3', order: order++,name6:'F1'}
    ]
    return {table:table}
  }
})
</script>

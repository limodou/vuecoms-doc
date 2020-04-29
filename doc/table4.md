# 表格行编辑

<div id="ex-table-04">
  <Row>
  <i-col span="1">&nbsp;</i-col>
  <i-col span="23">
  <Grid ref="table" :data="table">
    <h3 slot="beforeQuery" style="text-align:center">可编辑表格</h3>
  </Grid>
  </i-col>
  </Row>
</div>
<script>
var ex_table_04 = new Vue({
  el: '#ex-table-04',
  data: function () {
    var self = this
    var table = {
      editMode: 'row', // 行编辑模式
      nowrap: true,
      actionColumn: 'Action',
      indexCol: true,
      checkCol: true,
      multiSelect: true,
      static: false,
      pagination: true,
      total: 6,
      columns: [
        {name:'name1', title:'Name1', width:120, editor: {type: 'string', onChange: function(v, row){
            console.log(v, row)
          },
          onEnableEdit: function(value, column, row){
            if (row.id === 2) return false
            else return true
          }
        },
        },
        {name:'name2', title:'Name2', align: 'left', 
          editor: {type: 'select', static: true, options: {
          choices: [['A', 'Test A'], ['B', 'Test B']]
          }}
        },
        {name:'name3', title:'Name3', width:200, editor: {type: 'i-switch'}},
        {name:'name4', title:'Name4', width:200, format: function (value, column, row)   {
            return '<a href="#">' + value + '</a>'
          },
          showTitle: function(value) {
            return value
          },
          editor: {type: 'date', rule: {type: 'any', required: true}, 
          options: function(value, name, row) {
            return {
              options: {
                disabledDate: function (date) {
                  if (row.id === 1)
                    return date && date.valueOf() < Date.now() - 86400000;
                  else
                    return false
                }
              }
            }
          }}
        },
        {name:'name5', title:'Select', width: 150, editor: {type: 'select', labelField: 'name6', options: {
          filterable: true,
          remote: true,
          remoteMethod: function(term, callback){
            setTimeout(function(){
            callback([{label: 'Select A', value: 'A'}, {label: 'Select B', value: 'B'}, {label: 'Select C', value: 'C'}])
            },100)
          }
        }}},
        {name:'name6', title:'Name6', width: 150, hidden: false, editor: {type: 'str', static: true}},
        {name:'Action', title:'Name5', fixed: 'right', width: 120}
      ],
      buttons: [
        [
          {label: '新建', type:'primary', onClick: function(grid){
              grid.addEditRow({name2: 'A'})
            }
          }
        ],
        [{label: '查看结果', type:'primary', onClick: function(grid){
            console.table(grid.store.states.data)
          }}],
        [{label: '显示注释', type:'primary', onClick: function(grid){
              grid.setComment(4, 'name3', '这是评论')
            }},
        {label: '隐藏注释', type:'primary', onClick: function(grid){
              grid.removeComment(4, 'name3')
            }}
          ],
        [{label: '显示Class', type:'primary', onClick: function(grid){
              grid.setClass(3, 'name3', 'ivu-btn-error')
            }},
        {label: '删除Class', type:'primary', onClick: function(grid){
              grid.removeClass(3, 'name3')
            }}
          ],
        [
          {label: '切换样式', type: 'primary', onClick: function () {
            if (self.$refs.table.theme === 'default')
              self.$refs.table.theme = 'simple'
            else
              self.$refs.table.theme = 'default'
          }},
          {label: '静态切换', type: 'info', onClick: function () {
            self.$refs.table.static = !self.$refs.table.static 
          }}
        ],
        [
          {label: '隐藏按钮', type: 'primary', name: 'hiddenBtn', hidden: true},
          {label: '切换隐藏按钮', type: 'primary', onClick: function(grid, store){
            self.$set(grid.btns['hiddenBtn'], 'hidden', !grid.btns['hiddenBtn'].hidden)
          }},
        ]
      ],
      data: [],
      onSaveRow: function (row, callback) {
        self.$Message.info("save")
        if (row.name1 === 'ok') {
          setTimeout(function() {
            callback('ok', row)
          }, 500)
        } else {
          setTimeout(function() {
            callback('error', {name1: '不正确'})
          }, 500)
        }
      },
      onError: function (error) {
        self.$Message.error('数据有问题，请先修改')
      },
      onDeleteRow: function (row, callback) {
        self.$Message.info("delete")
        callback('ok', row)
      },
      onRowEditRender: function (h, row) {
        if (row.id === 3) {
          return h('div', '本行不可编辑')
        }
      }
    }
    table.data.push({id:1, name1:'Field-A1', name2:'A', name3:'Field-C1', name4:'Field-D1', name5: 'A', name6: 'Select A'})
    table.data.push({id:2, name1:'Field-A2', name2:'B', name3:'Field-C2', name4:'Field-D2', name5: 'B', name6: 'Select B'})
    table.data.push({id:3, name1:'Field-A3', name2:'A', name3:'Field-C3', name4:'Field-D3', name5: 'C', name6: 'Select C'})
    table.data.push({id:4, name1:'Field-A4', name2:'B', name3:'Field-C4', name4:'Field-D4', name5: 'D', name6: 'Select D'})
    table.data.push({id:5, name1:'Field-A5', name2:'A', name3:'Field-C5', name4:'Field-D5', name5: 'E', name6: 'Select E'})
    table.data.push({id:6, name1:'Field-A6', name2:'A', name3:'Field-C6', name4:'Field-D6', name5: 'F', name6: 'Select F'})
    return {table:table}
  }
})
</script>

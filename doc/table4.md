# 表格行编辑

<div id="ex-table-04">
  <Grid ref="table" :data="table"></Grid>
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
      columns: [
        {name:'name1', title:'Name1', width:200, editor: 'string'},
        {name:'name2', title:'Name2', width: 200, align: 'left', editor: 'select', editorOptions: {
          choices: [['A', 'Test A'], ['B', 'Test B']]
          }},
        {name:'name3', title:'Name3', width:200},
        {name:'name4', title:'Name4', width:200, editor: 'date'},
        {name:'Action', title:'Name5'}
      ],
      buttons: [
        [{label: '查看结果', type:'primary', onClick: function(){
            console.table(self.$refs.table.store.states.data)
          }}],
        [{label: '显示注释', type:'primary', onClick: function(){
              self.$refs.table.store.setComment(1, 'name3', '这是评论')
            }},
        {label: '隐藏注释', type:'primary', onClick: function(){
              self.$refs.table.store.removeComment(1, 'name3')
            }}
          ],
        [{label: '显示Class', type:'primary', onClick: function(){
              self.$refs.table.store.setClass(3, 'name3', 'ivu-btn-error')
            }},
        {label: '删除Class', type:'primary', onClick: function(){
              self.$refs.table.store.removeClass(3, 'name3')
            }}
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
      onDeleteRow: function (row, callback) {
        self.$Message.info("delete")
        callback('ok', row)
      }
    }
    table.data.push({id:1, name1:'Field-A1', name2:'A', name3:'Field-C1', name4:'Field-D1'})
    table.data.push({id:2, name1:'Field-A2', name2:'B', name3:'Field-C2', name4:'Field-D2'})
    table.data.push({id:3, name1:'Field-A3', name2:'A', name3:'Field-C3', name4:'Field-D3'})
    table.data.push({id:4, name1:'Field-A4', name2:'B', name3:'Field-C4', name4:'Field-D4'})
    table.data.push({id:5, name1:'Field-A5', name2:'A', name3:'Field-C5', name4:'Field-D5'})
    table.data.push({id:6, name1:'Field-A6', name2:'A', name3:'Field-C6', name4:'Field-D6'})
    return {table:table}
  }
})
</script>

# 复杂表格

<style>
.u-query{
  display: inline-block;
}
.memo{
  vertical-align: top;
  font-size: 14px;
  margin-bottom: 12px;
  height: 36px;
  line-height: 36px;
}
</style>
<div id="ex-table-02">
  <div>
    <input ref="loading" v-model="loading_text" style="display:inline-block"></input>
    <label style="display:inline-block">切换loading<input v-model="show_loading" type="checkbox"></input></label>
    <label style="display:inline-block">切换nowrap<input v-model="show_nowrap" type="checkbox"></input></label>
    <button @click="handleTitleHide">切换Title字段显示</button>
  </div>
  <Grid ref='grid' :data="table" :value="value"
    @on-selected="handleSelected"
    @on-deselected="handleDeselected"
    @on-selected-all="handleSelectedAll"
    @on-deselected-all="handleDeselectedAll"
    @on-query-change="handleQueryChange">
    <span slot="afterQuery" class="memo">备注</span>
    </Grid>
  <div>Selected: {{selected}}</div>
  <div>Param: {{param}}</div>
</div>
<script>
var ex_table_02 = new Vue({
  el: '#ex-table-02',
  data: function () {
    var self = this
    var order = 1
    var index = 1
    var value = []
    var table = {
      columns: [],
      multiSelect: true,
      resizable: true,
      pagination: true,
      pageSize: 50,
      pageSizeOpts: [10, 30, 50],
      total: 80,
      height: 300,
      draggable: true,
      checkCol: true,
      idField: 'id',
      orderField: 'order',
      clickSelect: true,
      checkColWidth: 120,
      checkColTitle: 'Check All',
      headerTitle: true,
      indexCol: true,
      // selectedRowClass: '',
      param: {
        str1: "Hello World!!!",
        tree: {label: 'children1', value: 'children1'}
      },
      buttons: [
        [
          {label: '新增', type:'primary', onClick: function(target, data){
              self.$Message.info('Click 新增')
            }
          },
          {label: '编辑', disabled: true},
          {label: '删除', onClick: function(target, data){
              var selection = target.getSelection()
              if (selection.length === 0) {
                self.$Message.error('请先选择要删除的记录')
              } else {
                target.removeRow(selection)
              }
            }
          },
          {label: '设置机构查询值', onClick: function(target, data){
              self.$refs.grid.$refs.query.store.states.value.tree = '2'
              self.$refs.grid.$refs.query.store.states.fields[1].options.label = 'A机构'
            }
          }
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
          {label: '清除所有选中', type: 'primary', onClick: function (target, store){
            store.deselectAll()
          }}
        ],
        [
          {label: '点击后消失', name: 'hiddenBtn', type: 'primary', onClick: function (target, store){
            self.$set(target.btns.hiddenBtn, 'hidden', true)
          }}
        ]
      ],
      rightButtons: {items: [
        [{label: '下载', size: 'small', type: 'primary'}]
      ], size: 'default'},
      bottomButtons: [
        [{'label': '导出'}]
      ],
      onLoadData: function (url, param, callback) {
        console.log(param)
        self.param = param
        var data = []
        for (var i = 0; i < param.pageSize; i++) {
          var row = {id: (param.page-1) * param.pageSize + i + 1, title: 'P' + param.page + '-Title-' + (i + 1), order: order++}
          for (var j = 1; j < 10; j++) {
            row['name' + j] = 'P' + param.page + '-Name-' + (i + 1) + '-' + j
          }
          data.push(row)
        }
        setTimeout( function () {
          callback(data, {total:1000000})
          }, 0)
      },
      onSelect: function (row) {
        var r = row.id !== 1
        if (!r) {
          self.$Message.info('本行不能选择')
        }
        return r
      },
      onCheckable: function (row) {
        var r = row.id !== 2 && row.id !==3
        return r
      },
      onMove: function(order, callback) {
        console.log('order', order)
        callback(true)
      }
    }
    table.columns.push({
      name: 'id',
      title: '合并/ID',
      width: 40,
      sortable: true,
      fixed: 'left'
    })
    table.columns.push({
      name: 'title',
      title: '合并/Very Long Title Test',
      sortable: true,
      fixed: 'left',
      showTitle: false,
      format: function(value, column, row) {
        return '<a href="#">' + value + '</a>'
      }
    })
    for (var j = 1; j < 10; j++) {
      table.columns.push({
        name: 'name' + j,
        title: 'Column' + j,
        width: 100,
        sortable: false,
        align: 'center'
      })
    }
    //隐藏字段
    table.columns.push({
      name: 'action',
      title: 'Action',
      sortable: false,
      fixed: 'right'
    })
    for (var i = 0; i < 10; i++) {
      var row = {id: i + 1, title: 'Title-' + (i + 1)}
      for (var j = 1; j < 10; j++) {
        row['name' + j] = 'Name-' + (i + 1) + '-' + j
      }
      value.push(row)
    }
    table.query = {
      firstLayoutAlign: 'left',
      fields: [
        {name: "str1", type: "string", label: "字符串1", placeholder: "请输入字符串1"},
        {name: "tree", type: "treeselect", label: "机构", options: {
          labelInValue: true,
          remote: true,
          'remote-load-data': function (item, callback) {
            if (!item) {
              callback([
                {
                  id: 'parent',
                  title: 'parent',
                  loading: false,
                  children: []
                }
              ])
            } else {
              callback([
                {
                    title: 'children1',
                    id: 'children1'
                },
                {
                    id: 'children2',
                    title: 'children2'
                }
              ])
            }
          }
        }
      }
      ],
      layout: [
        ['str1', 'tree']
      ],
      // buttons: [
      //   [{name: 'submit', label: '点击查询'}],
      //   [{name: 'download', label: '下载', onClick: function(target){
      //     self.$Message.info('点击了下载')
      //   }}],
      // ],
      choices: {}
    }
    return {
      table:table,
      selected:[],
      logs:[],
      loading_text:'loading',
      show_loading:false,
      show_nowrap: false,
      param:{},
      value: value
    }
  },
  watch: {
    show_loading: function() {
      this.$refs.grid.showLoading(this.show_loading, this.loading_text)
    },
    show_nowrap: function () {
      this.$refs.grid.$set(this.$refs.grid.store.states, 'nowrap', this.show_nowrap)
    }
  },
  mounted: function () {
    this.$refs.grid.store.setSelection([1, 2])
  },
  methods: {
    handleSelected: function(row) {
      this.selected = this.$refs.grid.getSelection()
      this.logs.push(['selected', row])
    },
    handleDeselected: function(row) {
      this.selected = this.$refs.grid.getSelection()
      this.logs.push(['deselected', row])
    },
    handleSelectedAll: function(row) {
      this.selected = this.$refs.grid.getSelection()
      this.logs.push(['selected-all', row])
    },
    handleDeselectedAll: function(row) {
      this.selected = this.$refs.grid.getSelection()
      this.logs.push(['deselected-all', row])
    },
    handleTitleHide: function() {
      var title_column = this.table.columns[1]
      this.$set(title_column, 'hidden', !title_column.hidden)
    },
    handleQueryChange: function (data) {
      console.log(data)
    }
  }
})
</script>

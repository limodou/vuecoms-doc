# 复杂表格

<div id="ex-table-02">
  <div>
    <input ref="loading" v-model="loading_text" style="display:inline-block"></input>
    <label style="display:inline-block">切换loading<input v-model="show_loading" type="checkbox"></input></label>
    <button @click="handleTitleHide">切换Title字段显示</button>
  </div>
  <Grid ref='grid' :data="table"
    @on-selected="handleSelected"
    @on-deselected="handleDeselected"
    @on-selected-all="handleSelectedAll"
    @on-deselected-all="handleDeselectedAll"></Grid>
  <div>Selected: {{selected}}</div>
  <div>Param: {{param}}</div>
</div>
<script>
var ex_table_02 = new Vue({
  el: '#ex-table-02',
  data: function () {
    var self = this
    var table = {
      columns: [],
      multiSelect: true,
      resizable: true,
      pagination: true,
      pageSizeOpts: [10, 30, 50],
      total: 80,
      height: 300,
      draggable: true,
      checkCol: true,
      checkColWidth: 120,
      checkColTitle: 'Check All',
      indexCol: true,
      data: [
      ],
      buttons: [
        [{label: '新增', type:'primary', onClick: function(){
            self.$Message.info('Click 新增')
          }}, {label: '编辑', disabled: true}, {label: '删除'}],
        [{label: '上移', icon:'ios-arrow-thin-up'}, {label: '下移', icon: 'ios-arrow-thin-down'}]
      ],
      rightButtons: [
        [{label: '下载'}]
      ],
      bottomButtons: [
        [{'label': '导出'}]
      ],
      onLoadData: function (url, param, callback) {
        self.param = Object.assign({}, param)
        var data = []
        var b = (param.page - 1) * param.pageSize
        for (var i = 0; i < param.pageSize; i++) {
          var row = {id: b + i + 1, title: 'P' + param.page + '-Title-' + (i + 1)}
          for (var j = 1; j < 10; j++) {
            row['name' + j] = 'P' + param.page + '-Name-' + (i + 1) + '-' + j
          }
          data.push(row)
        }
        setTimeout( function () {
          callback(data)
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
      title: '合并/Title',
      sortable: true,
      fixed: 'left',
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
      name: 'title',
      title: 'Title',
      sortable: false,
      hidden: true
    })
    for (var i = 0; i < 10; i++) {
      var row = {id: i + 1, title: 'Title-' + (i + 1)}
      for (var j = 1; j < 10; j++) {
        row['name' + j] = 'Name-' + (i + 1) + '-' + j
      }
      table.data.push(row)
    }
    table.query = {
      fields: [
        {name: "str1", type: "str", label: "字符串1", placeholder: "请输入字符串1"}
      ],
      layout: [
        ['str1']
      ],
      value: {
        str1: "Hello World!!!"
      },
      buttons: {
        align: "center",//按钮左中右 start center end 默认 end
        submit: {
          label: "点此查询",
        },
        clear: {
          label: "点此清除"
        }
      }
    }
    return {
      table:table,
      selected:[],
      logs:[],
      loading_text:'loading',
      show_loading:false,
      param:{}
    }
  },
  watch: {
    show_loading: function() {
      this.$refs.grid.showLoading(this.show_loading, this.loading_text)
    }
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
    }
  }
})
</script>

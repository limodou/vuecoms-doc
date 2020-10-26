# 简单表格

<div id="ex-table-01">
  <Tabs :animated="false">
    <tab-pane label="展示">
      <Grid :data="table" ref="grid"></Grid>
    </tab-pane>
    <tab-pane label="HTML">
      <pre>
        &lt;Grid :data="table" ref="grid"&gt;&lt;/Grid&gt;
      </pre>
    </tab-pane>
    <tab-pane label="Javascript">
      <pre>
var ex_table_01 = new Vue({
  el: '#ex-table-01',
  data: function () {
    var self = this
    var table = {
      nowrap: false,
      indexCol: true,
      theme: 'default',
      pagination: true,
      multiHeaderSep: '//',
      headerShow: true,
      hoverShow: false,
      sortMode: 'local',
      param: {sortField: 'name1', 'sortDir': 'asc'},
      total: 1,
      page: 1,
      columns: [
        {name:'check', title:'#', width:40, fixed: 'left', render: function(h, param) {
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
        {name:'name1', title:'Name1', width:100, fixed: 'left', sortable: true},
        {name:'name2', title:'Name2 Name2中中中 中中中中 <i class="ivu-icon ivu-icon-logo-youtube"></i>', width: 100},
        {name:'name3', title:'Name3', headerRender: function(h, param){
          return h('Button', {props: {type: 'primary', size: 'small'}, on: {click: function(){
            self.$Message.info('按钮点击事件')
          }}}, '自定义表头')
        }, width:100},
        {name:'name4', title:'Name4', align:'center', width:400, html: false},
        {name:'name5', title:'Name5', width:80, render: function(h, param) {
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
        {name:'name6', title:'Name6', fixed: 'right'}
      ],
      data: [],
      combineCols:[['check', 'name1', 'name2', 'name3'], ['name5']],
      buttons: [
        [
          {label: '切换样式', type: 'primary', onClick: function () {
            if (self.$refs.grid.theme === 'default')
              self.$refs.grid.theme = 'simple'
            else
              self.$refs.grid.theme = 'default'
          }}
        ],
        [
          {label: '切换表头', type: 'primary', onClick: function () {
            self.$refs.grid.store.states.headerShow = !self.$refs.grid.store.states.headerShow
          }}
        ]
      ]
    }
    table.data.push({id:1, check: 'A1', name1:'A1', name2:0, name3:'C1', name4:'<i>Hello</i>', name5:'E1', name6:'F1'})
    table.data.push({id:2, check: 'A1', name1:'A1', name2:'0', name3:'C1', name4:'D1', name5:'E1', name6:'F1'})
    table.data.push({id:3, check: 'A1', name1:'A1', name2:'0', name3:'C1', name4:'D1', name5:'E1', name6:'F1'})
    table.data.push({id:4, check: 'A2', name1:'A2', name2:'0', name3:'C1', name4:'D1', name5:'E2', name6:'F1'})
    table.data.push({id:5, check: 'A2', name1:'A2', name2:'0', name3:'C1', name4:0, name5:'E2', name6:'F1'})
    table.data.push({id:6, check: 'A1', name1:'A1', name2:'0', name3:'C1', name4:'D1', name5:'E3', name6:'F1'})
    table.data.push({id:7, check: 'A1', name1:'A1', name2:'0', name3:'C1', name4:'D1', name5:'E3', name6:'F1'})
    return {table:table}
  }
})
      </pre>
    </tab-pane>
  </Tabs>
</div>

<script>
var ex_table_01 = new Vue({
  el: '#ex-table-01',
  data: function () {
    var self = this
    var table = {
      nowrap: false,
      indexCol: true,
      theme: 'default',
      pagination: true,
      multiHeaderSep: '//',
      headerShow: true,
      hoverShow: false,
      sortMode: 'local',
      param: {sortField: 'name1', 'sortDir': 'asc'},
      total: 1,
      page: 1,
      columns: [
        {name:'check', title:'#', width:40, fixed: 'left', render: function(h, param) {
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
        {name:'name1', title:'Name1', width:100, fixed: 'left', sortable: true},
        {name:'name2', title:'Name2 Name2中中中 中中中中 <i class="ivu-icon ivu-icon-logo-youtube"></i>', width: 100},
        {name:'name3', title:'Name3', headerRender: function(h, param){
          return h('Button', {props: {type: 'primary', size: 'small'}, on: {click: function(){
            self.$Message.info('按钮点击事件')
          }}}, '自定义表头')
        }, width:100},
        {name:'name4', title:'Name4', align:'center', width:400, html: false},
        {name:'name5', title:'Name5', width:80, render: function(h, param) {
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
        {name:'name6', title:'Name6', fixed: 'right'}
      ],
      data: [],
      combineCols:[['check', 'name1', 'name2', 'name3'], ['name5']],
      buttons: [
        [
          {label: '切换样式', type: 'primary', onClick: function () {
            if (self.$refs.grid.theme === 'default')
              self.$refs.grid.theme = 'simple'
            else
              self.$refs.grid.theme = 'default'
          }}
        ],
        [
          {label: '切换表头', type: 'primary', onClick: function () {
            self.$refs.grid.store.states.headerShow = !self.$refs.grid.store.states.headerShow
          }}
        ]
      ]
    }
    table.data.push({id:1, check: 'A1', name1:'A1', name2:0, name3:'C1', name4:'<i>Hello</i>', name5:'E1', name6:'F1'})
    table.data.push({id:2, check: 'A1', name1:'A1', name2:'0', name3:'C1', name4:'D1', name5:'E1', name6:'F1'})
    table.data.push({id:3, check: 'A1', name1:'A1', name2:'0', name3:'C1', name4:'D1', name5:'E1', name6:'F1'})
    table.data.push({id:4, check: 'A2', name1:'A2', name2:'0', name3:'C1', name4:'D1', name5:'E2', name6:'F1'})
    table.data.push({id:5, check: 'A2', name1:'A2', name2:'0', name3:'C1', name4:0, name5:'E2', name6:'F1'})
    table.data.push({id:6, check: 'A1', name1:'A1', name2:'0', name3:'C1', name4:'D1', name5:'E3', name6:'F1'})
    table.data.push({id:7, check: 'A1', name1:'A1', name2:'0', name3:'C1', name4:'D1', name5:'E3', name6:'F1'})
    return {table:table}
  }
})
</script>

## 横向单元格合并

当单元格的取值为 `'--'` 时，将合并至前一个单元格上。与横向单元格合并相关的配置有：

* `colspan` 缺省是 `false`，当为 `true` 时启用横向合并
* `colspanDelimeter` 缺省是 `'--'`，可以换成其它的。表示此单元合将合并至前一个非 `'--'` 的单元格上。

以下示例的数据为：

```
        {name1: 'A1', name2: '--', name3: '--', name4: '1'},
        {name1: 'A1', name2: '--', name3: '--', name4: '2'},
        {name1: 'A1', name2: 'A2', name3: 'A3', name4: '3'},
        {name1: 'A1', name2: '--', name3: 'A3', name4: '4'},
        {name1: 'A1', name2: 'A2', name3: '--', name4: '5'},
```

<div id="ex-table-02">
  <Grid :data="data" ref="grid"></Grid>
</div>

<script>
var ex_table_02 = new Vue({
  el: '#ex-table-02',
  data: function () {
    var data = {
      colspan: true,
      columns: [
        {name:'name1', title:'Name1', width:100, align: 'left'},
        {name:'name2', title:'Name2', width: 100},
        {name:'name3', title:'Name3'},
        {name:'name4', title:'Name4'},
      ],
      combineCols:[['name1']],
      data: [
        {name1: 'A1', name2: '--', name3: '--', name4: '1'},
        {name1: 'A1', name2: '--', name3: '--', name4: '2'},
        {name1: 'A1', name2: 'A2', name3: 'A3', name4: '3'},
        {name1: 'A1', name2: '--', name3: 'A3', name4: '4'},
        {name1: 'A1', name2: 'A2', name3: '--', name4: '5'},
      ],
    }
    return {data:data}
  }
})
</script>

# 任务编辑

<div id="app">
  <build ref="build" :data="data" :value="value" :errors="errors" :rules="rules" :label-width="labelWidth"></build>
  <Modal v-model="openNewReq"
    title="对话框标题"
    width="1024"
    ref="dialog"
  >
    <Grid ref="dialogGrid" :data="reqData" :value="reqValue"></Grid>
  </Model>
</div>
<script>
Vue.component('team', {
    template: '<Grid ref="grid" :data="data" :value="value" @input="handleInput"><h3 slot="beforeQuery">小组角色</h3></Grid>',
    props: ['value'],
    data: function () {
      var self = this
      return  {
          data: {
            indexCol: true,
            checkColTitle: '',
            idField: 'sn',
            editMode: 'row',
            actionColumn: 'action',
            height: 150,
            columns: [
              {name: 'sn', title: 'ID', hidden: true},
              {name: 'name', title: '成员角色', width: 100, editor: {type: 'select'}},
              {name: 'user', title: '人员', width: 120, editor: {type: 'select', options: {
                  filterable: true, remote: true, rich: true, remoteMethod: function(query, callback){
                    setTimeout(function(){
                      if (query === 'a')
                        callback([{value:'A', label:'Test A', text: 'A'}, {value:'B', label:'Test B', text: 'B'}, {value:'C', label:'Test C', text: 'C'}])
                      else
                        callback([{value:'D', label:'Test D', text: 'D'}, {value:'E', label:'Test E', text: 'E'}, {value:'F', label:'Test F', text: 'F'}])
                      }, 1000)
                    },
                  },
                  onChange: function(v, row){
                    console.log(v)
                    self.$set(row, 'username', v.text || '')
                  }
                },
              },
              {name: 'username', title: '姓名', width: 100, editor: {static: true}},
              {name: 'members', title: '成员', editor: {type: 'select', multiple: true, options: {
                  filterable: true, remote: true, rich: true, remoteMethod: function(query, callback){
                    setTimeout(function(){
                      if (query === 'a')
                        callback([{value:'A', label:'Test A', text: 'A'}, {value:'B', label:'Test B', text: 'B'}, {value:'C', label:'Test C', text: 'C'}])
                      else
                        callback([{value:'D', label:'Test D', text: 'D'}, {value:'E', label:'Test E', text: 'E'}, {value:'F', label:'Test F', text: 'F'}])
                    }, 1000)
                  },
                  remoteSelected: function(v, callback){
                    setTimeout(function(){
                      if (v && v.length > 0)
                        callback([{label: 'Test A', value: 'A'}])
                    }, 1000)
                  },
                  onRenderLabel: function(item) {
                    return '<span class="item">' + item.text + ' - ' + item.label + '</span>'
                  },
                  onChanging: function(value, selected) {
                    console.log(value, selected)
                  }
                }
              }},
              {name: 'action', title: '操作', width: 250, render: function(h, param){
                var buttons = [
                  param.grid.defaultEditRender(h, param.row),
                  param.grid.defaultDeleteRender(h, param.row),
                ]
                if (!param.row._editting) {
                  buttons.push(h('Button', 
                    {
                      props: {
                        type: 'primary',
                        size: 'small',
                      },
                      style: {
                          margin: '0 5px'
                      },
                      on: {
                        click: function () {
                          param.grid.store.addEditRow({name: param.row.name, members: param.row.members})
                        }
                      }
                    }, '缺省增加')
                  )
                }
                return h('div', {
                  'class': 'u-cell-text'
                }, buttons)
              }}
            ],
            pagination: true,
            total: 1,
            buttons: [
              [{label: '增加角色', type:'primary', onClick: function(target, store){
                store.addEditRow(null)
              }}]
            ],
            onSaveRow: function (row, callback) {
              self.$Message.info("save")
              callback('ok', row)
              console.log('save', row)
            },
            onDeleteRow: function (row, callback) {
              self.$Message.info("delete")
              callback('ok', row)
              console.log('delete', row)
            }
          }
        }
      },
      methods: {
        handleInput: function (value) {
          this.$emit('input', value)
        }
      },
      mounted: function () {
        var self = this
        setTimeout(function(){
          var col = self.$refs.grid.getColumn('name')
          self.$set(col.editor, 'options', {filterable: true, choices: [['A', 'Test ATEST B TEST C test DTEST B TEST C test D'], ['B', 'Test B']]})
        }, 1000)
      }
    }
  )
Vue.component('mybox', {
  template: ['<Card style="margin-bottom:20px">',
  '<p slot="title" style="margin-bottom:0">基本信息（使用Card组件）</p>',
  '<i-button slot="extra" size="small">新增</i-button>',
  '<slot></slot>',
  '</Card>'].join('')
})
var ex_build_02 = new Vue({
  el: '#app',
  data: function () {
    var self = this
    var data = [
      {
        boxComponent: 'mybox',
        title: '基本信息',
        fields: [
          {name: 'title', label: '标题', placeholder: '请输入...', required: true},
          {name: 'desc', label: '说明', type: 'text', required: true, options: {
            autosize: {minRows: 5}
          }},
        ],
        layout: [
          ['title'],
          ['desc']
        ],
        labelDir: 'vertical'
      },
      {
        title: '关联业务要求',
        boxOptions: {headerClass: 'primary'},
        fields: [
          {name: 'busirequires', label: '', type: 'Grid', options: {
            data: {
              indexCol: true,
              checkColTitle: '',
              idField: 'sn',
              editMode: 'row',
              actionColumn: 'action',
              columns: [
                {name: 'sn', title: '业务要求编号', width: 150},
                {name: 'title', title: '业务要求名称', align: 'left'},
                {name: 'depart', title: '提出部门', width: 200},
                {name: 'contact', title: '联系人', width: 100},
                {name: 'action', title: '操作', width: 120, fixed: 'right', render: function (h, param){
                var buttons = [
                  param.grid.defaultEditRender(h, param.row),
                  // param.grid.defaultDeleteRender(h, param.row),
                ]
                return h('div', {
                  'class': 'u-cell-text'
                }, buttons)
                  // return h('i-button', {
                  //   on: {
                  //     click: function(value) {
                  //       console.log(param)
                  //     }
                  //   },
                  //   props: {
                  //     size: 'small',
                  //     type: 'error'
                  //   }
                  // }, '删除')
                }}
              ],
              pagination: true,
              total: 1,
              buttons: [
                [{label: '关联业务要求', type:'primary', onClick: function(){
                  self.openNewReq = !self.openNewReq
                  self.$refs.dialogGrid.deselectAll()
                }}]
              ],
            }
          }
          },
        ],
        layout: [
          ['busirequires'],
        ]
      },
      {
        title: '需求项分析结果',
        boxOptions: {headerClass: 'primary'},
        fields: [
          {name: 'reqitems', label: '', type: 'Grid', options: {
            data: {
              indexCol: true,
              checkColTitle: '',
              idField: 'sn',
              columns: [
                {name: 'sn', title: '业务要求编号', width: 150},
                {name: 'title', title: '业务要求名称', align: 'left'},
                {name: 'reqitemsn', title: '需求项编号', width: 150},
                {name: 'reqitemname', title: '需求项名称', width: 100},
                {name: 'type', title: '项目类型', width: 100},
                {name: 'prjname', title: '项目名称', width: 100},
                {name: 'action', title: '操作', width: 120, render: function (h, param){
                  return h('div', {
                  },
                  [
                    h('i-button', {
                      style: {
                        marginRight: '4px'
                      },
                      on: {
                        click: function(value) {
                          console.log(param)
                        }
                      },
                      props: {
                        size: 'small',
                        type: 'error'
                      }
                    }, '删除'),
                    h('i-button', {
                      on: {
                        click: function(value) {
                          console.log(param)
                        }
                      },
                      props: {
                        size: 'small',
                        type: 'primary'
                      }
                    }, '关联项目')
                  ])
                }}
              ],
              pagination: true,
              total: 1,
              buttons: [
                [
                  {label: '导入需求项分析结果', type:'primary', onClick: function(){
                  }},
                  {label: '导入模板下载', type:'default', onClick: function(){
                  }}
                ]
              ],
            }
          }
          },
        ],
        layout: [
          ['reqitems'],
        ]
      },
      {
        title: '小组成员',
        boxOptions: {headerClass: 'primary'},
        fields: [
          {name: 'team', label: '', type: 'team'}
        ],
        layout: [
          ['team'],
        ]
      },
    ]
    var reqData = {
      width: 988,
      height: 150,
      checkCol: true,
      indexCol: true,
      pagination: true,
      idField: 'sn',
      editMode: 'row',
      actionColumn: 'action',
      buttons: [
        [
          {label: '新建', type:'primary', onClick: function(target, store){
              store.addEditRow({name2: 'A'}, true)
            }
          }
        ],
      ],
      columns: [
        {name: 'sn', title: '业务要求编号', width: 150},
        {name: 'title', title: '业务要求名称'},
        {name: 'depart', title: '提出部门', width: 200},
        {name: 'contact', title: '联系人', width: 100, editor: {type: 'select', options: {
            choices: [['A', '张三'], ['B', '李四']]
          }}
        },
        {name: 'action', title: '操作', width: 120}
      ],
      onLoadData: function (url, param, callback) {
        var data = [
          {sn: 'Test001', title: 'AAAAA', depart: 'BBBBB', contact: 'A'},
          {sn: 'Test002', title: 'AAAAA', depart: 'BBBBB', contact: 'A'},
          {sn: 'Test003', title: 'AAAAA', depart: 'BBBBB', contact: 'A'},
          {sn: 'Test004', title: 'AAAAA', depart: 'BBBBB', contact: 'A'},
          {sn: 'Test005', title: 'AAAAA', depart: 'BBBBB', contact: 'A'},
          {sn: 'Test006', title: 'AAAAA', depart: 'BBBBB', contact: 'A'},
          {sn: 'Test007', title: 'AAAAA', depart: 'BBBBB', contact: 'A'},
        ]
        setTimeout( function () {
          callback(data, {total:100})
          }, 0)
      },
    }
    return {data:data, 
            value: {
              busirequires: []
            },
            reqData: reqData,
            reqValue: [],
            errors: {},
            rules: {},
            labelWidth: 150,
            openNewReq: false
          }
  },
  mounted: function () {
    var self = this
    setTimeout(function() {
      self.value.busirequires = [
        {sn: 'Test001', title: 'AAAAA', depart: 'BBBBB', contact: 'CCCCC'},
        {sn: 'Test002', title: 'AAAAA', depart: 'BBBBB', contact: 'CCCCC'},
        {sn: 'Test003', title: 'AAAAA', depart: 'BBBBB', contact: 'CCCCC'},
        {sn: 'Test004', title: 'AAAAA', depart: 'BBBBB', contact: 'CCCCC'},
        {sn: 'Test005', title: 'AAAAA', depart: 'BBBBB', contact: 'CCCCC'},
      ]
    }, 1000)
    // setTimeout(function() {
    //  self.reqValue = [
    //    {sn: 'Test001', title: 'AAAAA', depart: 'BBBBB', contact: 'A'}
    //  ]
    //}, 1000)
    setTimeout(function() {
      self.$set(self.value, 'team', [
          {sn: '1', name: 'A', user: {label: 'Test A', value: 'A'}, members: [{label: 'Test A', value: 'A'}, {label: 'Test B', value: 'B'}] },
          {sn: '2', name: 'B', user: {label: 'Test C', value: 'C'}, members: [{label: 'Test A', value: 'A'}, {label: 'Test C', value: 'C'}] }
        ]
      )
    }, 1000)
  },
  methods: {
    save: function(error) {
      if (error) {
        this.$Message.error(error)
      } else {
        this.$Message.info('saved')
      }
    }
  }
})
</script>
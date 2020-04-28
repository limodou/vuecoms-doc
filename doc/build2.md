# 多段展示

Build控件可以展示多个段落。

<div id="ex-build-02">
  <build ref="build" :data="data" :value="value" 
    :errors="errors" :rules="rules" :label-width="labelWidth"
    theme="tab" :show-box="false" :buttons="buttons"></build>
  <div>
    {{value}}
  </div>
</div>
<script>
var choices = [
  {label:'选项一', value: 'A'},
  {label:'选项二', value: 'B'},
  ]
Vue.component('u-table', {
  template: '<table><tr><th>Test</th></tr><tr><td>AAA</td></tr></table>'
})
var ex_build_02 = new Vue({
  el: '#ex-build-02',
  data: function () {
    var self = this
    var data = [
      {
        name: 'basic1',
        title: '基本信息1',
        theme: 'border',
        labelAlign: 'left',
        fields: [
          {name: 'str1', label: '字符串1', placeholder: '请输入...', help: '帮助信息',
            info: 'info信息', required: true, rule: {type: 'email'}, labelAlign: 'right'},
          {name: 'str2', label: '静态字符串2', static: true, required: true, convert: function(v){
            return '<a href="#">' + v + '</a>'
            }
          },
          {name: 'select1', label: '选择', type: 'select', required: true, options: {choices: choices}},
          {name: 'select2', label: '选择', type: 'select', static: true, options: {choices: choices}},
          // {name: 'rich1', label: '富文本', type: 'tinymce', options: { options: {height: 200}}},
          {name: 'grid', label: '表格', type: 'Grid', labelDir: 'vertical', options: {
            data: {
              columns: [
                {name:'name1', title:'Name1', width:100, fixed: 'left'},
                {name:'name2', title:'Name2', width: 100, fixed: 'left'},
                {name:'name3', title:'Name3', width:100},
                {name:'name4', title:'Name4', align:'center', width:200},
              ]
            }
          }}
        ],
        layout: [
          ['str1', 'str2'],
          ['select1', 'select2'],
          // ['rich1'],
          ['grid']
        ],
      },
      {
        name: 'files',
        title: '附件',
        component: 'u-table'
      },
      {
        name: 'basic2',
        title: '基本信息2',
        fields: [
          {name: 'select3', label: '选择', type: 'select', required: true, multiple: true, options: {choices: choices}
          },
          {name: 'select4', label: '选择', type: 'select', multiple: true, static: true, options: {choices: choices}
          },
        ],
        layout: [
          ['select3', 'select4'],
        ]
      },
    ]
    return {data:data, value: {
              select1: 'B',
              select2: 'A',
              select3: ['A', 'B'],
              select4: ['A', 'B'],
              grid: [
                {name1: '1', name2: '2', name3: '3', name4: '4'},
                {name1: '2', name2: '2', name3: '3', name4: '4'},
                {name1: '3', name2: '2', name3: '3', name4: '4'},
                {name1: '4', name2: '2', name3: '3', name4: '4'},                
              ]
            },
            errors: {},
            rules: {
              str1: function(rule, value, callback, source, options) {
                if (value !== 'abc@gmail.com') {
                  callback (new Error('邮件地址必须为 abc@gmail.com'))
                } else {
                  callback()
                }
              }
            },
            buttons: [
              [{label: '查看结果', type:'primary', onClick: function(target, data){
                  console.log(target, data)
                }
              }],
              [{label: '校验', type:'primary', onClick: function(target, data){
                  self.$refs.build.validate(self.save)
                }
              }],
              [{label: '合并出错结果', type:'info', onClick: function(target, data){
                  self.$refs.build.errors = {select1: '这是合并后的错误'}
                }
              }],
              [{label: '隐藏基本信息2', type:'info', onClick: function(target, data){
                  //self.$refs.build = {select1: '这是合并后的错误'}
                  self.$set(self.$refs.build.data[2], 'hidden', !self.$refs.build.data[2].hidden)
                }
              }],
              [
                {component: 'u-select', props: {choices: [{label: 'Test A', value: 'A'}, {label: 'Test B', value: 'B'}], value: 'A'}, on: {input: function(v){self.$Message.info(v)}}}
              ]
            ],
            labelWidth: 200
          }
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

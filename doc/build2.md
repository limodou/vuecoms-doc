# 多段展示

<div id="ex-build-02">
  <build ref="build" :data="data" :value="value" :errors="errors" :rules="rules" :label-width="labelWidth"></build>
  <div>
    {{value}}
  </div>
</div>
<script>
function choices1_options (callback) {
  setTimeout(function () {
    var c = [
      {label:'选项一', value: 'A'},
      {label:'选项二', value: 'B'},
      {label:'选项三', value: 'C'}
    ]
    callback(c)
  }, 1000)
}
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
        fields: [
          {name: 'str1', label: '字符串1', placeholder: '请输入...', help: '帮助信息',
            info: 'info信息', required: true, rule: {type: 'email'}},
          {name: 'str2', label: '静态字符串2', static: true, required: true, convert: function(v){
            return '<a href="#">' + v + '</a>'
            }
          },
          {name: 'select1', label: '选择', type: 'select', required: true, options: {choices: choices1_options}
          },
          {name: 'select2', label: '选择', type: 'select', static: true, options: {choices: choices1_options}
          },
        ],
        layout: [
          ['str1', 'str2'],
          ['select1', 'select2'],
        ],
        component: 'Layout',
        boxComponent: 'Box',
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
          {name: 'select3', label: '选择', type: 'select', required: true, multiple: true, options: {choices: [
            {label:'选项一', value: 'A'},
            {label:'选项二', value: 'B'},
            ]}
          },
          {name: 'select4', label: '选择', type: 'select', multiple: true, static: true, options: {choices: [
            {label:'选项一', value: 'A'},
            {label:'选项二', value: 'B'},
            ]}
          },
          {name: 'radio1', label: '选择', type: 'radio', required: true, multiple: true, options: {choices: [
            {label:'选项一', value: 'A'},
            {label:'选项二', value: 'B'},
            ]}
          },
          {name: 'radio2', label: '选择', type: 'radio', static: true, options: {choices: [
            {label:'选项一', value: 'A'},
            {label:'选项二', value: 'B'},
            ]}
          },
          {name: 'checkboxgroup1', label: '选择', type: 'checkboxgroup', required: true, options: {choices: [
            {label:'选项一', value: 'A'},
            {label:'选项二', value: 'B'},
            ]}
          },
          {name: 'checkboxgroup2', label: '选择', type: 'checkboxgroup', static: true, options: {choices: [
            {label:'选项一', value: 'A'},
            {label:'选项二', value: 'B'},
            ]}
          },
          {name: 'checkbox1', label: '选择', type: 'checkbox', required: true},
          {name: 'checkbox2', label: '选择', type: 'checkbox', static: true},
          {name: 'text1', label: '文本1', type: 'text', required: true, options: {autosize: false}},
          {name: 'text2', label: '文本2', type: 'text', static: true},
          {name: 'date1', label: '日期1', required: true, type: 'date'},
          {name: 'date2', label: '日期2', type: 'date', static: true},
          {name: 'tree1', label: '树选择', required: true, type: 'treeselect', multiple: true, options: {options:
            [ {
                  id: 'fruits',
                  label: 'Fruits',
                  children: [ {
                    id: 'apple',
                    label: 'Apple',
                  }, {
                    id: 'grapes',
                    label: 'Grapes',
                  }, {
                    id: 'pear',
                    label: 'Pear',
                  }, {
                    id: 'strawberry',
                    label: 'Strawberry',
                  }, {
                    id: 'watermelon',
                    label: 'Watermelon',
                  } ],
                }, {
                  id: 'vegetables',
                  label: 'Vegetables',
                  children: [ {
                    id: 'corn',
                    label: 'Corn',
                  }, {
                    id: 'carrot',
                    label: 'Carrot',
                  }, {
                    id: 'eggplant',
                    label: 'Eggplant',
                  }, {
                    id: 'tomato',
                    label: 'Tomato',
                  } ],
                } ]
              }
          },
        ],
        layout: [
          ['select3', 'select4'],
          ['radio1', 'radio2'],
          ['checkboxgroup1', 'checkboxgroup2'],
          ['checkbox1', 'checkbox2'],
          ['text1'],
          ['text2'],
          ['date1', 'date2'],
          ['tree1']
        ],
        buttons: {
          items: [
            [{label: '查看结果', type:'primary', onClick: function(target, data){
                console.log(target, data)
              }
            }],
            [{label: '校验', type:'primary', onClick: function(target, data){
                target.validate(self.save)
              }
            }],
            [{label: '合并出错结果', type:'info', onClick: function(target, data){
                self.errors = {select1: '这是合并后的错误'}
              }
            }]
          ],
          size: 'default'
        }
      }
    ]
    return {data:data, value: {
              select1: 'B',
              select2: 'A',
              select3: ['A', 'B'],
              select4: ['A', 'B'],
              radio2: 'A',
              checkboxgroup2: ['A', 'B'],
              checkbox2: 'B',
              text1: 'Line 1\nLine 2',
              text2: 'Line 3\nLine 4',
              date2: '2017-12-12'
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

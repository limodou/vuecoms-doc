# 基础控件说明

为了更好实现界面生成，vuecoms对iview的控件进行了一定的包装及扩展，以下为常见的字段类型。

<div id="ex-build-01">
  <build ref="build" :data="data" :value="value" :choices="choices"></build>
  <div>
    {{value}}
  </div>
</div>
<script>
var ex_build_01 = new Vue({
  el: '#ex-build-01',
  data: function () {
    var self = this
    var data = [
      {
        name: 'basic',
        title: '基本信息',
        labelWidth: 300,
        fields: [
          {name: 'str1', label: '输入框-string', placeholder: '请输入...', static: false, help: '帮助信息',
            info: 'info信息', required: true, rule: {type: 'email'}},
          {name: 'select1', label: '选择-select', type: 'select', required: true, options: {clearable: true}},
          {name: 'select1_1', label: '远程选择-select', type: 'select', options: {
            clearable: true,
            filterable: true, 
            remote: true, 
            rich: true, 
            remoteMethod: function(query, callback){
              setTimeout(function(){
                if (query === 'a')
                  callback([{value:'A', label:'Test A', text: 'A'}, {value:'B', label:'Test B', text: 'B'}, {value:'C', label:'Test C', text: 'C'}])
                else
                  callback([{value:'D', label:'Test D', text: 'D'}, {value:'E', label:'Test E', text: 'E'}, {value:'F', label:'Test F', text: 'F'}])
                }, 1000)
              }
            }
          },
          {name: 'select4', label: '多选-select', type: 'select', multiple: true, static: true, options: {choices: [
            {label:'选项一', value: 'A'},
            {label:'选项二', value: 'B'},
            ]}
          },
          {name: 'select5', label: '可创建单选-select', type: 'select', multiple: false, options: {
            allowCreate: true,
            filterable: true,
            onCreateItem (query) {
              return {label: query, value: query, other: '123'}
            },
            choices: [
            {label:'选项一', value: 'A'},
            {label:'选项二', value: 'B'},
            ]}
          },
          {name: 'select6', label: '可创建多选-select', type: 'select', multiple: true, options: {
            allowCreate: true,
            filterable: true,
            onCreateItem (query) {
              return {label: query, value: query, other: '123'}
            },
            choices: [
            {label:'选项一', value: 'A'},
            {label:'选项二', value: 'B'},
            ]}
          },
          {name: 'radio1', label: 'Radio选择-radio', type: 'radio', required: true, options: {choices: [
            {label:'选项一', value: 'A'},
            {label:'选项二', value: 'B'},
            {label: '不可选项', value: 'C', disabled: true}
            ]}
          },
          {name: 'checkboxgroup1', label: 'Checkbox多选-checkboxgroup', type: 'checkboxgroup', multiple: true, required: true, options: {choices: [
            {label:'选项一', value: 'A'},
            {label:'选项二', value: 'B'},
            {label: '不可选项', value: 'C', disabled: true}
            ]}
          },
          {name: 'checkbox1', label: '单选框-checkbox', type: 'checkbox', required: true},
          {name: 'text1', label: '多行文本-text', type: 'text', required: true, rule:{max:20}, help: '最多输入20个汉字'},
          {name: 'date1', label: '日期-date', required: true, type: 'date', rule: 'date', options: {type: 'year', convert: true}},
          {name: 'date3', label: '日期时间-datetime', required: true, type: 'datetime', rule: {type: 'date'}, options: {convert: true}},
          {name: 'daterange1', label: '日期范围-datepickerrange', required: true, type: 'datepickerrange', rule: {type: 'array'},
          options: {
            type: 'datetime',
            convert: false,
            width: 160,
            disabledBegin: function(d) {
              var now = new Date()
              now.setHours(0)
              now.setMinutes(0)
              now.setSeconds(0)
              now.setMilliseconds(0)
              return d < now
            }
          }},
          {name: 'tree1', label: '树选择-treeselect', required: true, type: 'treeselect', multiple: true, options: {
            checkStrictly: true,
            onlyLeaf: false,
            labelInValue: true,
            choices:
            [ {
                  id: 'fruits',
                  title: 'Fruits',
                  children: [ {
                    id: 'apple',
                    title: 'Apple',
                  }, {
                    id: 'grapes',
                    title: 'Grapes',
                  }, {
                    id: 'pear',
                    title: 'Pear',
                  }, {
                    id: 'strawberry',
                    title: 'Strawberry',
                  }, {
                    id: 'watermelon',
                    title: 'Watermelon',
                  } ],
                }, {
                  id: 'vegetables',
                  title: 'Vegetables',
                  children: [ {
                    id: 'corn',
                    title: 'Corn',
                  }, {
                    id: 'carrot',
                    title: 'Carrot',
                  }, {
                    id: 'eggplant',
                    title: 'Eggplant',
                  }, {
                    id: 'tomato',
                    title: 'Tomato',
                  } ],
                } ]
              }
          },
          {name: 'switch', label: '开关-Switch', type: 'i-switch'},
          {name: 'slider', label: '滑块-Slider', type: 'Slider', required: true, rule: {type: 'number'}},
          {name: 'input1', label: '输入-Input', type:'Input', options: {search: true, "enter-button": '查询'},
            on: {'on-search': function () {
              self.$Message.info('on-search')
            },
            'on-focus': function () {
              self.$Message.info('on-focus')
            }}
          },
          {name: 'autocomplete', label: '自动完成-autocomplete', type: 'AutoComplete', options: {
            transfer: true,
            remoteMethod: function (query, callback) {
              callback(['Alabama', 'Alaska', 'Arizona', 'Arkansas', 'California',
                'Colorado', 'Connecticut', 'Delaware', 'District of columbia', 'Florida'])
            },
            filterMethod: function (v, item) {
              if (!v) return true
              return item.toUpperCase().indexOf(v.toUpperCase()) > -1
            }
          }},
          {name: 'utext', label: '计数多行文本-u-text', type: 'u-text', required: true, 
            showError: false,
            static: false,
            staticComponent: '',
            options: {count: 50, placeholder: '请输入文本'}}
        ],
        boxComponent: '',
      },
    ]
    return {
            data:data,
            value: {
              tree1: [],
              utext: 'abc\ndef'
            },
            choices: {
                select1: [
                  {label:'选项一', value: 'A'},
                  {label: '不可选项', value: 'B', disabled: true}
                ]
            },
            errors: {},
            rules: {
            },
          }
  },
})
</script>

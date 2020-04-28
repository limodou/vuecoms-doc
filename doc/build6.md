# 自定义控件

Build支持用户自定义组件，定义好通过 `Vue.component('name', component)` 注册，然后在定义field时，type属性就是控件名称即可。为了与Build进行交互，自定义控件需要实现v-model的接口，即：props中含有value参数，在数据发生变化时需要抛出 input 事件。同时要注意，控件本身应该对value的变化进行处理，以更现在外部对数据进行修改时即时更新，因此有可能需要避免因监听value变化而导致的死循环问题（可以在内部保存数据，然后与外部的value值进行比较，当比较不一致时才继续处理）。

一个自定义组件主要考虑以下处理：

1. props 要实现 value 参数，并处理它的变化
2. 数据变化时要抛出 input 事件
3. 如果考虑组件分为可编辑，静态展示的不同场景，可以在 props 添加 static 或 mode 来区分不同的处理模式，在外部通过 options 传入即可
4. 如果是单个组件，可以使用 Build 统一的校验来处理，不需要特别处理。
5. 如果是复杂组件，可以按例二和例四的示例来处理，示例分别展示了复杂组件和复杂数组组件的例子。

作为自定义组件，Build会传入三个参数：value, validateResult, field。其中value是当前字段的值，validateResult是整个Build的校验对象，field是当前自定义字段对象。

例三展示了自定义布局，适用于原本按layout来自动布局的处理，改为局部自定义布局。

## 示例一，单一组件

本示例展示了一个简单的自定义组件，手机验证码的组件。不过这些示例未展示 value 作为输入参数的情形，只是用于输出 value。

<div id="ex-build-00">
  <build ref="build" :data="data" :value="value"></build>
  <div>{{value}}</div>
</div>
<script>
Vue.component('mobile-verify', {
  template: [
    '<Row>',
    '<Col span="8"><Input :value="val" @input="handleInput"></Input></Col>',
    '<Col span="5" offset="1"><Button type="primary" @click="handleClick" :loading="loading">{{btnText}}</Button></Col>',
    '</Row>'].join(''),
  props: ['value', 'validateResult', 'field'],
  mounted: function () {
    var self = this
    this.validateResult[this.field.name].children = function (rule, value) {
      if (!self.verifyCode) return '请获取验证码'
      if (self.verifyCode && (value !== self.verifyCode)) return '验证码不正确'
    }
  },
  data: function () {
    return {val: '', verifyCode: '', btnText: '获取验证码', loading: false, timer: 60}
  },
  methods: {
    timeout: function () {
      if (this.timer === 58) {
        this.verifyCode = '123456'
        this.$Message.info('验证码为: '+this.verifyCode)
        this.btnText = '获取验证码'
        this.timer = 60
        this.loading = false
      } else {
        this.loading = true
        this.timer --
        this.btnText = this.timer
        setTimeout(this.timeout, 1000)
      }
    },
    handleClick: function () {
      this.loading = true
      this.timeout()
    },
    handleInput: function (value) {
      this.val = value
      this.$emit('input', value)
    }
  },
  watch: {
    value: function (v) {
      this.val = v
    }
  }
})
var ex_build_00 = new Vue({
  el: '#ex-build-00',
  data: function () {
    var self = this
    var data = [
      {
        name: 'basic',
        title: '自定义组件示例一',
        labelWidth: 150,
        fields: [
          {name: 'must', label: '是否必填', type: 'checkbox', onChange: function (value) {
            self.$set(self.$refs.build.fields.verifyCode, 'required', !self.$refs.build.fields.verifyCode.required)
          }},
          {name: 'verifyCode', label: '手机验证码', type: 'mobile-verify'},
        ],
        // layout: [
        //   ['verifyCode']
        // ],
        buttons: [
          [{label: '查看结果', type:'primary', onClick: function(target, data){
              target.validate().then(res => {
                console.log(res)
                self.$Message.success('校验成功')
              })
              // .catch(err => {
              //   self.$Message.error(err)
              // })
              console.log(target, data)
            }
          }],
          [{label: '重置', type:'primary', onClick: function(target, data){
              target.reset()
            }
          }]
        ],
      },
    ]
    return {data:data, value: {}}
  }
})
</script>

## 示例二，复杂组件

本例演示了自定义证件号码，证件类型为一个字段的情况，对应于value是一个对象的形式，如： `{type: '', no: ''}`。如果要定义这种形式，主要解决以下问题：

1. 字段定义
2. 校验处理
3. 展示处理

通过引入 FormBlock 组件，它提供了 makeFields 和 makeValidateResult 函数，分别用于生成 Build 需要的字段结构和校验对象。因为要引用 FormBlock 组件的实例，所以示例是在 mounted 中进行处理的，通过 $refs 来引用实例。本例需要定义一个内部的 validateResult 对象，在本例中为 validateResult1，它将作为对应字段缺省校验对象中的 children 属性，如代码： `this.validateResult[this.field.name].children = this.validateResult1` 所示。

对于字段的展示，使用了 FormCell 组件，它自带 label，字段展示和出错显示。

<div id="ex-build-01">
  <build ref="build" :data="data" :value="value" :choices="choices" :errors="errors" ></build>
  <div>{{value}}</div>
</div>
<script>
Vue.component('credentials', {
  template: [
    '<form-block ref="block">',
    '<Row v-if="fields">',
    '<Col span="12"><form-cell :col="fields[\'type\']" :value="value" :validateResult="validateResult1"></form-cell></Col>',
    '<Col span="12"><form-cell :col="fields[\'no\']" :value="value" :validateResult="validateResult1"></form-cell></Col>',
    '</Row>',
    '</form-block>'].join(''),
  props: ['value', 'validateResult', 'field'],
  mounted: function () {
    var self = this
    var root = this.$refs.block
    this.fields = root.makeFields([
      {name: 'type', label: '证件类型', labelWidth: 150, type: 'select', required: true, options: {choices: [
        {label: '身份证', value: '01'},
        {label: '军官证', value: '02'},
      ]}},
      {name: 'no', label: '证件号码', labelWidth: 150, required: true, rule: 'idnumber'}
    ])
    this.validateResult1 = root.makeValidateResult(this.fields)
    this.validateResult[this.field.name].children = this.validateResult1
  },
  data: function () {
    return {fields: null, validateResult1: {}}
  },
})
var ex_build_01 = new Vue({
  el: '#ex-build-01',
  data: function () {
    var self = this
    var data = [
      {
        name: 'basic',
        title: '自定义组件示例二',
        labelWidth: 150,
        fields: [
          {name: 'select1', label: '选择1', type: 'select', required: true, options: {clearable: true,
              choices: [
                {label:'选项一', value: 'A'},
                {label:'选项二', value: 'B'},
              ] 
            },
            onChange: function (value, alldata) {
              self.$set(alldata, 'select2', value)
            }
          },
          {name: 'select2', label: '选择2', type: 'select', static: true},
          {name: 'custom', label: '', type: 'credentials', rule: 'any', showError: false}
        ],
        layout: [
          ['select1', 'select2'],
          ['custom']
        ],
        buttons: {
          items: [
            [{label: '查看结果', type:'primary', onClick: function(target, data){
                target.validate().then(res => {
                  console.log(res)
                  self.$Message.success('校验成功')
                }).catch(err => {
                  self.$Message.error(err)
                })
                console.log(target, data)
              }
            }],
            [{label: '重置', type:'primary', onClick: function(target, data){
                target.reset()
              }
            }]
          ],
        }
      },
    ]
    return {
            data:data,
            value: {
              select1: '',
              select2: '',
              custom: {type: '01', no: ''},
            },
            choices: {
              // select1: [],
              select2: []
            },
            errors: {},
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
  },
  mounted: function () {
    var self = this
    setTimeout(function () {
      self.$set(self.value, 'select2', 'A')
    }, 50)
    setTimeout(function () {
      var c = [
        {label:'选项一', value: 'A'},
        {label:'选项二', value: 'B'},
        {label:'选项三', value: 'C'}
      ]
      self.$set(self.choices, 'select2', c)
    }, 1000)
  }
})
</script>

## 示例三，自定义布局组件

对于不想采用缺省布局展示的场合，可以考虑使用自定义布局。通过定义一个自定义布局组件，并在原Build中的layout进行声明，如：

```
layout: [
  ['select1', 'select2'],
  [{component: 'customlayout', fields: ['type', 'no']}]
],
```

第三行就是一个自定义布局的示例。它对应的组件为 customlayout ，通过 component 来定义。对应两个字段 'type', 'no'。

使用自定义布局是不需要改变数据组构，即不需要组件为一个对象，只是布局上调整，此时不适合使用自定义组件。

自定义布局组件接受四个参数：value 为 Build的值， validateResult 为校验对象，col为在layout中自动生成的对象（如果是字符串会转为对应的布局对象），fields为布局要处理的字段对象。定义fields时，用的是数组，每项都是对应字段的name值。转为参数时，变成对象形式，其值是Build中fields对应的字段对象值。在本例中使用 fields['type']和fields['no'] 来分别引用证件类型和证件号码两个字段。

<div id="ex-build-02">
  <build ref="build" :data="data" :value="value" :choices="choices" :errors="errors" ></build>
  <div>{{value}}</div>
</div>
<script>
Vue.component('customlayout', {
  template: [
    '<div style="border: 1px solid #ddd;padding: 15px 0;">',
    '<div style="margin-left: 10px; font-size: 12px;">证件信息：</div>',
    '<Row style="margin-bottom: 10px;">',
    '<Col span="12"><form-cell :col="fields[\'type\']" :value="value" :validateResult="validateResult"></form-cell></Col>',
    '</Row>',
    '<Row>',
    '<Col span="12"><form-cell :col="fields[\'no\']" :value="value" :validateResult="validateResult"></form-cell></Col>',
    '</Row>',
    '</div>'].join(''),
  props: ['value', 'validateResult', 'col', 'fields']
})
var ex_build_02 = new Vue({
  el: '#ex-build-02',
  data: function () {
    var self = this
    var data = [
      {
        name: 'basic',
        title: '自定义组件示例三',
        labelWidth: 150,
        fields: [
          {name: 'select1', label: '选择1', type: 'select', required: true, options: {clearable: true,
              // choices: [
              //   {label:'选项一', value: 'A'},
              //   {label:'选项二', value: 'B'},
              // ] 
            },
            onChange: function (value, alldata) {
              self.$set(alldata, 'select2', value)
            }
          },
          {name: 'select2', label: '选择2', type: 'select', static: true},
          {name: 'type', label: '证件类型', labelWidth: 150, type: 'select', required: true, options: {choices: [
            {label: '身份证', value: '01'},
            {label: '军官证', value: '02'},
          ]}},
          {name: 'no', label: '证件号码', labelWidth: 150, required: true, rule: 'idnumber'}
        ],
        layout: [
          ['select1', 'select2'],
          [{component: 'customlayout', fields: ['type', 'no']}]
        ],
        buttons: {
          items: [
            [{label: '查看结果', type:'primary', onClick: function(target, data){
                target.validate().then(res => {
                  console.log(res)
                  self.$Message.success('校验成功')
                }).catch(err => {
                  self.$Message.error(err)
                })
                console.log(target, data)
              }
            }],
            [{label: '重置', type:'primary', onClick: function(target, data){
                target.reset()
              }
            }]
          ],
        }
      },
    ]
    return {
            data:data,
            value: {
              select1: '',
              select2: '',
              type: '01',
              no: '',
            },
            choices: {
              select1: [],
              select2: []
            },
            errors: {},
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
  },
  mounted: function () {
    var self = this
    setTimeout(function () {
      self.$set(self.value, 'select1', 'A')
    }, 50)
    setTimeout(function () {
      var c = [
        {label:'选项一', value: 'A'},
        {label:'选项二', value: 'B'},
        {label:'选项三', value: 'C'}
      ]
      self.$set(self.choices, 'select1', c)
      self.$set(self.choices, 'select2', c)
    }, 1000)
  }
})
</script>

## 示例四，复杂数组组件

<div id="ex-build-03">
  <build ref="build" :data="data" :value="value" :choices="choices" :errors="errors" ></build>
  <div>{{value}}</div>
</div>
<style>
.form-block {
  border: 1px solid whitesmoke; padding-top: 10px;
}
.form-block:hover {
  border: 1px solid #7dc1f5;
}
</style>
<script>
Vue.component('credentials2', {
  template: [
    '<form-block ref="block" class="form-block">',
    '<Row v-if="fields" v-for="(v, index) of value" style="margin-bottom: 10px;">',
    '<Col span="8"><form-cell :col="fields[\'type\']" :value="value[index]" :validateResult="validateResult1[index]"></form-cell></Col>',
    '<Col span="10"><form-cell :col="fields[\'no\']" :value="value[index]" :validateResult="validateResult1[index]"></form-cell></Col>',
    '<Col span="5" offset="1">',
    '<Button type="primary" v-if="index===0" @click.prevent="handleAdd">增加证件</Button>',
    '<Button type="error" v-if="index>0" @click.prevent="handleDelete(index)">删除</Button>',
    '</Col>',
    '</Row>',
    '</form-block>'].join(''),
  props: ['value', 'validateResult', 'field'],
  mounted: function () {
    var self = this
    var root = this.$refs.block
    this.fields = root.makeFields([
      {name: 'type', label: '证件类型', labelWidth: 150, type: 'select', required: true, 
        options: {choices: [
          {label: '身份证', value: '01'},
          {label: '军官证', value: '02'},
        ]},
        onChange: function(value){
         console.log('change', value)
        },
        enableOnChange: true
      },
      {name: 'no', label: '证件号码', labelWidth: 150, required: true, rule: 'idnumber'}
    ])
    this.validateResult1 = [
      root.makeValidateResult(this.fields),
    ]
    this.validateResult[this.field.name].children = this.validateResult1
  },
  data: function () {
    return {fields: null, validateResult1: []}
  },
  methods: {
    handleAdd: function() {
      var root = this.$refs.block
      this.value.push({type: '', no: ''})
      this.validateResult1.push(root.makeValidateResult(this.fields))
    },
    handleDelete: function (index) {
      this.value.splice(index, 1)
      this.valudateResult1.splice(index, 1)
    }
  }
})
var ex_build_03 = new Vue({
  el: '#ex-build-03',
  data: function () {
    var self = this
    var data = [
      {
        name: 'basic',
        title: '自定义组件示例四',
        labelWidth: 150,
        fields: [
          {name: 'title', label: '名称', required: true, rule: {type: 'string', required: true, trim: true}},
          {name: 'nation', label: '国籍', type: 'radio', required: true, 
            onChange: function (value, model) {
              console.log(value, model)
              var required = false
              if (value === '中国') required = true
              self.$set(self.$refs.build.fields['location'], 'required', required)
              self.$set(self.$refs.build.fields['zhmiao'], 'required', required)
              self.$refs.build.setFieldRule('location')
              self.$refs.build.setFieldRule('zhmiao')
            },
            options: {
              choices: [
                {label:'中国', value: '中国'},
                {label:'其它', value: '其它'},
              ],
            }
          },
          {name: 'custom', label: '', type: 'credentials2', required: true, rule: {validate: function(rule, value, model) {
            console.log('校验')
          }}, showError: true},
          {name: 'location', label: '籍贯', required: false},
          {name: 'zhmiao', label: '政治面貌', required: false},
        ],
        layout: [
          ['title'],
          ['nation'],
          ['custom'],
          [{name: 'location', colspan: 8}],
          [{name: 'zhmiao', colspan: 8}]
        ],
        buttons: {
          items: [
            [{label: '查看结果', type:'primary', onClick: function(target, data){
                target.validate().then(res => {
                  if (!res) {
                    self.$Message.success('校验成功')
                  }
                }).catch(err => {
                  self.$Message.error(err)
                })
                console.log(target, data)
              }
            }],
            [{label: '重置', type:'primary', onClick: function(target, data){
                target.reset()
              }
            }]
          ],
        }
      },
    ]
    return {
            data:data,
            value: {
              select1: '',
              select2: '',
              custom: [{type: '', no: ''}],
            },
            choices: {
              select1: [],
              select3: []
            },
            errors: {},
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
  },
  mounted: function () {
    var self = this
    setTimeout(function () {
      self.$set(self.value, 'select1', 'A')
    }, 50)
    setTimeout(function () {
      var c = [
        {label:'选项一', value: 'A'},
        {label:'选项二', value: 'B'},
        {label:'选项三', value: 'C'}
      ]
      self.$set(self.choices, 'select1', c)
    }, 1000)
  }
})
</script>

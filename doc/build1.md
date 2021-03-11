# 基本构建

<div id="ex-build-01">
  <build ref="build" :data="data" :value="value" :errors="errors" 
  :rules="rules" :choices="choices" :box-options="boxOptions"></build>
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
        labelWidth: 150,
        staticSuffix: '_static',
        static: false,
        fields: [
          {name: 'str1', label: '字符串1', placeholder: '请输入...', static: false, help: '帮助信息',
            info: 'info信息', required: true, rule: {type: 'email'}, options: {appendText:'单位'}},
          {name: 'str2', label: '静态字符串2', static: true, required: true, format: function(v){
            return '<a href="#">' + v + '</a>'
          }},
          {name: 'select1', label: '选择1', type: 'select', required: true, options: {clearable: true,disabled: true}},
          {name: 'select2', label: '选择2', type: 'select', static: true, options: {clearable: true},
            help: '这是设置了select2_static的结果，未使用缺省机制'
          },
          {name: 'select1_1', label: '远程选择1', type: 'select', options: {
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
          {name: 'select3', label: '选择3', type: 'select', required: true, multiple: true, options: {choices: [
            {label:'选项一', value: 'A'},
            {label:'选项二', value: 'B'},
            ],
            onChanging: function(value, selected) {
              if (value === 'A' && !selected) {
                self.$Message.info("Can't removed")
                return false
              }
              return true
            }},
          },
          {name: 'select4', label: '选择4', type: 'select', multiple: true, static: true, options: {choices: [
            {label:'选项一', value: 'A'},
            {label:'选项二', value: 'B'},
            ]}
          },
          {name: 'radio1', label: '选择5', type: 'radio', required: true, options: {choices: [
            {label:'选项一', value: 'A'},
            {label:'选项二', value: 'B'},
            ]}
          },
          {name: 'radio2', label: '选择6', type: 'radio', static: true, options: {choices: [
            {label:'选项一', value: 'A'},
            {label:'选项二', value: 'B'},
            ]}
          },
          {name: 'checkboxgroup1', label: '选择7', type: 'checkboxgroup', multiple: true, required: true, options: {choices: [
            {label:'选项一', value: 'A'},
            {label:'选项二', value: 'B'},
            ]}
          },
          {name: 'checkboxgroup2', label: '选择8', type: 'checkboxgroup', static: true, options: {choices: [
            {label:'选项一', value: 'A'},
            {label:'选项二', value: 'B'},
            ]}
          },
          {name: 'checkbox1', label: '选择9', type: 'checkbox', required: true},
          {name: 'checkbox2', label: '选择10', type: 'checkbox', static: true},
          {name: 'text1', label: '文本1', type: 'text', required: true, rule: {max:20}, help: '最多输入20个汉字'},
          {name: 'text2', label: '文本2', type: 'text', static: true, rule:{max:20}, help: '最多输入20个汉字'},
          {name: 'date1', label: '日期1', required: true, type: 'date', rule: 'date', options: {type: 'year', convert: true}},
          {name: 'date2', label: '日期2', type: 'date', static: true},
          {name: 'date3', label: '日期3', required: true, type: 'datetime', rule: {type: 'date'}, options: {convert: true}},
          {name: 'date4', label: '日期4', type: 'datetime', static: true},
          {name: 'daterange1', label: '日期范围1', required: true, type: 'datepickerrange', rule: {type: 'array'},
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
          {name: 'daterange2', label: '日期范围2', required: true, type: 'datepickerrange', options: {type: 'year'}, rule: {type: 'array'}},
          {name: 'tree1', label: '树选择', required: true, type: 'treeselect', multiple: true, 
          onChange: function (value) {
              console.log('tree-select', value)
            },
            options: {
            checkStrictly: true,
            onlyLeaf: true,
            labelInValue: true,
            parentDisabledWhenOnlyLeaf: false,
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
                    disabled: true
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
          {name: 'switch', label: '开关', type: 'i-switch'},
          {name: 'slider', label: '滑块', type: 'Slider', required: true, rule: {type: 'number'}},
          {name: 'input1', label: '输入', type:'Input', options: {search: true, "enter-button": '查询'},
            on: {'on-search': function () {
              self.$Message.info('on-search')
            },
            'on-focus': function () {
              self.$Message.info('on-focus')
            }}
          },
          {name: 'autocomplete', label: '自动完成', type: 'AutoComplete', options: {
            transfer: true,
            remoteMethod: function (query, callback) {
              callback(['Alabama', 'Alaska', 'Arizona', 'Arkansas', 'California',
                'Colorado', 'Connecticut', 'Delaware', 'District of columbia', 'Florida'])
            },
            filterMethod: function (v, item) {
              if (!v) return true
              return item.toUpperCase().indexOf(v.toUpperCase()) > -1
            }
          }}
        ],
        layout: [
          ['str1', 'str2'],
          ['select1', 'select2'],
          [{name: 'select1_1', clospan: 12}],
          ['select3', 'select4'],
          ['radio1', 'radio2'],
          ['checkboxgroup1', 'checkboxgroup2'],
          ['checkbox1', 'checkbox2'],
          ['text1'],
          ['text2'],
          ['date1', 'date2'],
          ['date3', 'date4'],
          ['daterange1', 'daterange2'],
          ['tree1'],
          ['switch', 'slider'],
          ['input1', 'autocomplete']
        ],
        boxComponent: 'Box',
        boxOptions: {widthBorder: false, headerClass: 'primary'},
        buttons: {
          items: [
            [{label: '查看结果', type:'primary', onClick: function(target, data){
                console.log(target, data)
              }
            }],
            [{label: '校验', type:'primary', onClick: function(target, data){
                //target.validate(self.save) 旧的写法，使用回调
                //以下为新的写法，使用promise
                target.validate().then(function(){
                  self.save()
                }).catch(function(error){
                  self.save(error)
                })
              }
            }],
            [{label: '部分校验', type:'primary', onClick: function(target, data){
                //target.validate(self.save) 旧的写法，使用回调
                //以下为新的写法，使用promise
                target.validate({fields: ['str1', 'checkboxgroup1']}).then(function(){
                  self.save()
                }).catch(function(error){
                  self.save(error)
                })
              }
            }],
            [{label: '重置', type:'info', onClick: function(target, data){
                target.reset()
              }
            }],
            [{label: '合并出错结果', type:'info', onClick: function(target, data){
                self.errors = {select1: '这是合并后的错误'}
              }
            }],
            [{label: '更新select1-2 choices', type:'info', onClick: function(target, data){
                target.fields.select1.options.choices = [
                  {label:'选项A', value: 'A'},
                  {label:'选项B', value: 'B'},
                  {label:'选项C', value: 'C'}
                ]
                target.fields.select2.options.choices = [
                  {label:'选项A', value: 'A'},
                  {label:'选项B', value: 'B'},
                  {label:'选项C', value: 'C'}
                ]
              }
            }],
            [{label: '提交测试', type:'info', name: 'submit', onClick: function(target, data){
                var submit = target.btns.submit
                submit.disabled = true
                submit.loading = true
                setTimeout(function () {
                  submit.loading = false
                  submit.disabled = false
                }, 5000)
              }
            }],
            [{label: '点击隐藏按钮', name: 'hideBtn', onClick: function(target){
              var btn = target.btns.hideBtn;
              self.$set(btn, 'hidden', true)
            }}]
          ],
          size: ''
        }
      },
      {
        name: 'basic',
        title: '',
        labelWidth: 150,
        staticSuffix: '_static',
        fields: [
          {name: 'str1', label: '字符串1', placeholder: '请输入...', help: '帮助信息',
            info: 'info信息', required: true, rule: {type: 'email'}},
        ],
        layout: [
          ['str1']
        ],
        boxComponent: ''
      }
    ]
    return {
            data:data,
            value: {
              str1: '',
              str2: 'aaa',
              select1: 'A',
              select2_static: '静态结果',
              select1_1: {label: '选择X', value: 'X'},
              select3: ['A', 'B'],
              select4: ['A', 'B'],
              radio1: 'A',
              radio2: 'A',
              checkboxgroup2: ['A', 'B'],
              checkbox2: 'B',
              text1: 'Line 1\nLine 2',
              text2: 'Line 3\nLine 4\nLine 5\nLine 6\nLine 7\nLine 8',
              date1: '',
              date2: '2017-12-12',
              date3: '2017-12-12 12:01:28',
              date4: '2017-12-12 12:01:28',
              daterange1: ['2018-06-03 14:23:00', '2018-06-05 13:10:15'],
              tree1: [{label: 'Apple', value: 'apple', level: 2}, {label: 'Grapes', value: 'grapes', level: 3}]
            },
            choices: {
                select1: [
                  {label:'选项一', value: 'A'}
                ]
            },
            errors: {},
            rules: {
              str1: function(rule, value, source) {
                if (value !== 'abc@gmail.com') {
                  return '邮件地址必须为 abc@gmail.com'
                }
              }
            },
            boxOptions: {
              openIcon: 'ios-arrow-down',
              closeIcon: 'ios-arrow-forward'
            }
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
    /* var self = this
    setTimeout(function () {
      var c = [
        {label:'选项一', value: 'A'},
        {label:'选项二', value: 'B'},
        {label:'选项三', value: 'C'}
      ]
      self.$set(self.choices, 'select1', c)
    }, 1000) */
  }
})
</script>


## 参数说明

| 属性 | 说明 | 缺省值 |
|-----|-----|-------|
| data | Build参数，结构为一个Object，详细格式见下面 |  |
| labelWidth | 标签宽度 | 150px |
| staticSuffix | 静态字段后缀，当字段定义为static时，缺省先找形式为 name_suffix 的值 | '_static' |
| value | 传入的数据 | {} |
| errors | 错误信息，方便从外部传入。key为字段名 | {} | 
| rules | 单独定义的校验规则。key为字段名。它是在字段上定义的rule的一个补充 | {} |
| choices | select字段的choices值。方便从外部传入 | {} |
| showBox | 是否显示 Box 组件 | true |
| buttons | 独立于 section(段落中定义的 buttons)，为整个 Build 的按钮区。 | [] |
| theme | 风格。目前只支持两种，缺省的 'default' 和 'tab' 方式。 | default |
| labelDir | 标签是水平还是垂直布局（horizental 或 vertical) | horizental |
| labelAlign | 标签文字对齐方式，为 : left, right, center | right |
| statusObject | 状态对象，用于字段定义时指定 showWhen 时对应的对象。详见 [输入输出转换](./build9.md) | null |


## 方法说明

| 方法名 | 说明 | 返回值 |
|----------|-----|------|
| validate | 对Build的数据进行校验。支持回调模式 `validate(callback)` ,其中 `callback(error)` ，如果error为undefined时表示成功，error有值时，为最早一个出错信息。promise模式，可以在 `then()` 或 `catch((error)=>{})` 中继续处理成功或出错。<br/>`validate(callback)`还可以定义为`validate(options)`，options 是一个对象，可以包含：callback 或 fields。callback 就是回调函数。fields 为待校验的字段名数组。当你不需要校验全部字段时，可以使用种校验方式。| Promise |
| reset | 重置表单 | 无 |
| setFieldRule | 重置某个字段的校验规则，参数为 name 字段名 | 无 |
| validateField | 校验某个字段，参数为 name 字段名 | 无 |

## Validate说明

Build内置了一个校验组件，使用它可以方便对Build中的组件进行校验。在Build中使用validate支持两种定义方式：

1. 在定义Field时定义，如： `{name: 'field', rule: {type: 'string'}}`
2. 定义一个rules，作为与data平级的参数传入Build，格式为：

    ```
    rules: {
      field: {type: 'string', min: 4}
    }
    ```

### 规则定义

规则 (rule) 可以支持几种方式：字符串，对象，校验函数和数组。

* 字符串形式：就是规则的类型，如：{name: 'field', rule: 'string'}
* 对象形式：一个规则一般可以包含以下内容：

    * type 表示类型名称
    * validate 表示校验函数，形式为 `(rule, value, model)` 其中 rule 为规则对象本身，value为待校验的字段值，model为整个数据对象，可以用来检查除待校验字段以外的值。它可以返回以下几种类型：
    
        * 成功直接返回，相当于返回 undefined, null 为空的值
        * 出错信息描述字符串，如: `'出错信息'`
        * 出错异常如：`new Error('出错信息')`
        * Promise 对象，用于异步处理模式，其中 `resolve()` 表示成功， `reject('出错信息')` 表示出错
    * 规则的其它参数，不同的类型参数不同，需要参考不同的类型定义
    * `makeError(msgid, expected, actual)` 函数，用于生成对应的msgid对应的错误信息，后面再详细说明
    * messages 用于类型规则中，可以替换或增加新的出错信息模板
* 校验函数: 如同上面的 validate 函数，用于自定义的校验，又没有注册类型的时候。可以直接转一个函数对象。
* 数组是用于有多个校验时，每项值可以是上面类型的组合

### 规则类型

为了方便使用，validate定义了一些常见类型，并且有些还提供了对应的参数。如果要使用这些类型和参数，在定义规则时应使用对象形式，以字符串校验为例：

```
{
  type: 'string',
  min: 6
}
```

上面例子表示类型为字符串，最小长度需要为6。

预定义类型说明：

any 表示任意类型

  * required 必输项，不能为空值

array 表示数组类型

  * required 必输项，长度不能为0
  * min 最小长度需要大于等于此值
  * max 最大长度需小于等于此值
  * length 长度必须等于此值
  * contains 数组需要包含某个值，注意是一个值
  * enum 数组需要包含某个数组

boolean 表示布尔类型

  * convert 表示是否在比较前先进行数据转换，些转换不影响返回的结果，对于非布尔类型的值可以是 `1, 0, "1", "0", "true", "false", "on", "off"`，缺省为不自动转换
  
  如果 convert 不为true，则必须是布尔类型才能通过校验

date 表示日期类型，包括时分秒

  * convert 表示是否在比较前先进行数据转换。转换时目前是使用 `new Date(d)` 的方式。缺省为自动转换
  * required 必填校验

email 校验字符串是否是邮件格式，缺省是采用非精确校验，即有 `@` 符基本上就认为是邮件，精确方式对校验合法字符及域名要包含 `.`

  * required 必填校验
  * precise 精确校验

enum 校验当前值是否在某个数组中

  * required 必填校验
  * values 为待检查的数组

number 校验数值类型

  * convert 表示是否在比较前先进行数据转换。缺省不自动转换。
  * min 当前值要大于等于最小值
  * max 当前值要小于等于最大值
  * range 当前值要在range范围内，它是一个数组，如： `[2, 5]`
  * equal 当前值要等于equal的值
  * notequal 当前值不能待于notequal的值
  * integer 当前值需要是一个整数
  * positive 当前值需要是一个正数
  * negative 当前值需要是一个负数

object 校验对象类型

  * strict 检查当前对象必须包含 props 中对应的key值
  * props 这个需要与strict联动，它本身是一个对象，需要使用它的keys对当前值的key进行检查

string 校验字符串类型

  * trim 处理前先去除前后空格
  * required 必填校验, 校验时目前未自动执行 trim
  * min 最小长度需要大于等于此值
  * max 最大长度需小于等于此值
  * length 长度必须等于此值
  * range 当前值要在range范围内，它是一个数组，如： `['2', '5']`
  * pattern 使用正则表示式对当前值进行匹配，它可以是字符串或正则表示式对象。如果是字符串，则自动转为正则表达式，如果还有正则表示式的参数，可以使用 patternFlags 来设置
  * contains 检查当前值是否包含contians所给出的子串
  * enum 检查当前值是否为在enum给出的字符串的子串
  * numeric 检查当前值是否为数值，可以带负号和小数点
  * integer 检查当前值是否只包含数字
  * alpha 检查当前值是否为大小写字母
  * alphanum 检查当前值是否为大小写字母和数字
  * alphadash 检查当前值是否为大小写字母，数字，减号和下划线

url 检查是否为链接，检查是否是以 http:// 或 https:// 开始的字符串

  * required 必填校验

idnumber 检查是否为身份证号18位

  * required 必填校验

mobile 检查是否为手机号

  * required 必填校验

telephone 检查是否为固定电话号码

  * required 必填校验

ip 校验是否为IP地址

  * required 必填校验
  * ipv4 校验是否IPV4
  * ipv6 校验是否IPV6

password 校验密码

  * required 必填校验
  * min 最小长度需要大于等于此值
  * max 最大长度需小于等于此值
  * range 密码长度要在range范围内，它是一个数组，如： `[8, 20]`

realname 校验姓名

  * required 必填校验
  * hz 是否必须是汉字，否则可以是全汉字，或全英文+数字，及中英文的点

socialCreditCode 社会统一信用代码 [参考](https://zh.wikisource.org/zh-hans/GB_32100-2015_%E6%B3%95%E4%BA%BA%E5%92%8C%E5%85%B6%E4%BB%96%E7%BB%84%E7%BB%87%E7%BB%9F%E4%B8%80%E7%A4%BE%E4%BC%9A%E4%BF%A1%E7%94%A8%E4%BB%A3%E7%A0%81%E7%BC%96%E7%A0%81%E8%A7%84%E5%88%99)

  * required 必填校验

zipcode 邮政编码

  * required 必填校验

## 调用已有校验类型

如果想调用内置已有的类型校验方法，可以如下定义 zipcode 的校验：

```
async function (rule, value, model) {
  const r = Object.assign({}, rule, {type: 'string', length: 6, integer: true})
  let res
  try {
    res = await this.validateRule(r, value, model)
  } catch (e) {
    res = e
  }
  if (res)
      return r.makeError('zipcode')
}
```

注意不要使用箭头函数，因为validator会自动向函数中注入this对象。

原来的rule中会有一些注入的属性，如：

* field 字段名
* fullfield 字段显示名
* makeError 生成出错信息的函数

所以通过Object.assign可以将计划使用的类型和原注入的规则属性进行合并，以生成目标类型。

通过使用async/await用以实现Promise的处理，结果res如果有值，则表示出错，可以直接返回原类型校验的错误，也可以返回新生成的错误。

除使用async/await外，也可以使用Promise，返回时应返回Promise对象。

使用Promise的代码如下：

```
function (rule, value, model) {
  const r = Object.assign({}, rule, {type: 'string', length: 6, integer: true})
  return new Promise((resolve, reject) => {
    this.validateRule(r, value, model).then((res) => {
      if (res)
        resolve(r.makeError('zipcode'))
      else
        resolve()
    }).catch((err)=>{
      reject(r.makeError('zipcode'))
    })
  })
}
```

为了处理只是对现有校验类型进行包装的简单情况，还可以使用 `useRule` 方法，如 zipcode 的校验可以改为：

```
async function (rule, value, model) {
  return this.useRule({type: 'string', length: 6, integer: true}, rule, value,model, (err)=>{
    return rule.makeError('zipcode')
  })
}
```

useRule 第一个参数是目标校验类型的参数，后面 rule, value, model 都是原来校验函数传入的，箭头函数用于返回出错信息。这里使用了rule.makeError，也可以直接返回出错信息。

## 在内置规则基础之上包装新的类型

利用上面的 `validateRule` 或 `useRule` 可以在某个基本的类型之上封装新的类型，如 zipcode 就是在 string 基础之上

## 出错信息模板

当校验出错时，如果是自定义的校验函数，可以直接返回出错信息，但是对于内置或自定义的类型，为了更好的反映出错及i18n的要求，还需要返回翻译过或合成了参数及值的出错描述，所以给每一个出错的说明定义了一个message信息，目前是放在 `validator/messages` 下，目前缺省提供了中文和英文的翻译，只是包含内置的类型。

出错信息大概的格式为：

```
stringMin: "{field} 长度需大于等于 {expected} 长度！",
```

一个出错信息会有三个参数：

* field 表示字段汉字名称
* expected 期望的值
* actual 实际值

其中field是由validate框架给出，并且会将字段名转为汉字。expected, actual是在内置的校验函数中提供的。所以当你自已写新的类型时要自已定义msgid及对应的期望值和实际值。

## 集成说明

集成主要包括：

1. 初始化
2. 扩展新的校验类型及定制出错信息

如果不对validate进行任何修改，你不需要特别的初始化。vuecoms会自动初始化。

如果要进行扩展和定制，首先可以模仿validate的方式编写新的校验类型，同时定义新的消息模板，然后将其组织为一个对象，如：

```
import newtype from './newtype'
{
  rules: {
    type: newtype
  },
  messages: {
    消息标识: '消息模板 {field}'
  }
}
```

在 main.js 中引入 vuecoms ，然后初始化：

```
import Vue from 'vue'
import Vuecoms from 'vuecoms'
import customRules from 'customRules'

Vue.use(vuecoms, customRules)
```

## 旧版本兼容性

1. 对于 Vuecoms 2.X 及以上版本，自定义rules方法为 `(rule, value, callback, source, options)` 而2.X之后简化为： `(rule, value, model)` 。
2. 以前在校验出错时，需要使用 callback ，如： `callback (new Error('出错信息'))`, 如果成功则返回 `callback()` 。对于2.X，成功直接 `return`, 出错可以 `return '出错信息'` 或 `return new Error('出错信息')`。
3. 同时原来不支持自定义类型，现在则支持，并且可以使用 `rule.makeError(msgid, expected, actual)` 来调用错误生成方法，使用预定义的出错模板。
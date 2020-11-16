# 修改记录

## V3.7.4

* RadioGroup, CheckboxGroup, Select 增加对 disabled 的支持。在 choices 中指定 disabled 即可。
## V3.7.3

* Grid 增加 onBeforeEditing 事件回调，当返回 true 时可以继续编辑，否则直接退出
## V3.7.2

* Grid 增加 onEditing 和  onCancelEdit 事件回调，分别对应进入行编辑状态和取消行编辑状态的事件处理。
  
## V3.7.1

* Grid 增加单元格横向合并的功能,详情参见简单表格示例

## V3.7
* Grid parseUrl缺省值改为 false
* Grid 实现表头自定义渲染
* Grid 增加 isEditing 函数，返回当前是否存在处于行编辑的行

## V3.6.1
* 优化 Dialog 在删除时，执行 $destroy 的处理

## V3.6
* 调整 Build Label 在对齐时的星号位置
* Query 增加 setValue ，可以直接赋初始值
* Grid 增加 validateRow(row) 用于校验行编辑的数据
* Grid 增加当无数据时，向前翻页或刷新处理
* Grid 修复当 addRow 时，使用 before 时没有插在最前面的问题
* Grid 增加 onRowClass 的回调用于设置 tr 的属性
* validator 优化自定义错误消息的处理
* 优化 upload clear 方法，增加 emit 参数，用于指示是否抛出事件

## V3.5
* 修复 Grid 在 tree 模式下，checkbox 选中状态的处理 BUG
* 修复 Grid 的 onSaveRow 在行编辑处理时，未将回调返回的数据与正在编辑的数据进行合并的 BUG
* 扩充 Dialog 和 ConfirmDialog 的参数，使其基本与 iview 一致
* 优化 List.group 公共方法，增加 visit 的回调
* 优化 Select 的 onCreateItem 可以是 Promise
* 修复 checkbox 静态展示不正确的 BUG
* 优化 Build 中 string 的校验属性 trim 缺省生效
* 优化 Build 中字段生成的 key 的机制，为自动生成
* 优化 Build 的 date 组件中的日期格式可以使用 options.format 的设置，而不再写死
* 修复 Grid 在作为 Build 的嵌入组件时，当 value 发生变化时，对于已经存在的 selectedRows 的数据处理不正确的 BUG
* 修复分页组件跳转页号 Input 可以输入字母的 BUG
* 增加 Grid 表头列可以配置过滤器的功能，通过 filterable 属性，类似于行编辑 editor 的配置方式
* 增加 flat-select 组件，支持多选用于过滤器中
* 将 grid 等配置独立出来，可以通过 vue.$vuecomsConfig 来进行覆盖，进行定制化处理
* 优化了 clickoutside 对多元素的绑定，用在过滤器的 dropdown 弹出框里
* 对 query 的展示收起的文本也可以定制，可以用于客户化
* 对 vuecoms 提供的 input 组件增加对原生 iview 的事件的处理，通过 $listeners

## V3.4
* Grid theme 新增 mini 风格，去掉表头的灰色背景，同时增加 zebra 参数，用控制是否显示表格行斑马线，缺省为 true
* 修复 Toast 点击关闭时，会调用两次 onClose 的 BUG
* 在 Vue.prototype 上增加 $Dialog 和 $ConfirmDialog 方法，用于打开对话框，详情参见对话框说明
* Build 字段定义中的 type 可以直接传 component 对象，不用声明成全局的了。例如：

    ```
    import MyList form '@/src/components/MyList'

    ...
    fields: [
      {name: 'mylist', type: MyList}
    ]
    ```

* 对于 staticField 的处理，会将内容(display)中的 tag 去除
* 优化 flatChoices 组件，对多选进行了处理
* 修复了 Cell 在处理 onEnableEdit 时的参数错误
* Grid 在调用 onSaveRow 时，增加了第三个参数，第一个是干净的数据，第二个是原始的数据。主要是原始的数据对于表格行，可能有些_开头的数据是有特殊作用的，比如_rowKey
* deepCopy 解决循环的问题
* 校验规则 realname 增加 EN 的参数
* 修复树型表格，当一级节点为不可选中时，全选状态不正确的BUG
* select 组件的 rich 改为 labelInValue 保持和 iview 一致。原 rich 还保留

## V3.3

优化 Buttons 的处理。它是用在 Build 和 Grid 中的。通过某个 button 定义一个 onClick(root, data, btn) 回调，其中第一个参数或者是 Build 或 Grid 实例，可以使用 root.btns.name 来引用某个按钮。前提是定义按钮时给出了 name 属性。比如

```
{label: '保存', name: 'submit', onClick: function(build) {
  build.btns.submit
}}
```

上面 build.btns.submit 就可以拿到 submit 按钮的信息。可以设置按钮的 disabled, loading, hidden 属性。建议使用 $set 来设置，以防未在初始化时提供相应的占位变量。

## V3.2

* Build 的 validate() 进行优化。参数原来是 callback(可以缺省，采用 Promise 方式处理)，现在可以是一个对象。为了与原来兼容，这个参数可以包含 `callback` 或 `fields`。新增的 fields 是用于只校验部分字段。它是一个数组，用于指定要校验字段的 name 名字。
* Build 增加 labelAlign 参数，分别可以在 build, section, field 上进行定义。
* Build 优化 labelDir 参数，可以在 field 上进行定义。

## V3.1

* 优化表格操作列(Action)的处理。当处于编辑状态(row._editing，只会存在新增或修改时)时，会新创建保存和取消的按钮，而不是复用原有按钮。解决只有一个按钮时的问题。
* Build增加mapConvert/inputConvert/outputConvert的支持，可以在表单初始化数据和获取表单数据时，对数据进行转换，可以方便集成在field的定义中。
* Grid, Buttons的回调事件取消在调用时对this的绑定，目的是为了可以使用methods中定义的方法使用this对象。
* 优化input，增加对静态模式的支持。

## V3.0

* 增加Build对数据转入及转出的支持，详情 [参见](build9.md)
* 表格行编辑增加在点击保存按钮时，进行行校验的功能。可以在editor上定义rule。并且提供onError回调，可以处理出错信息。详情 [参见](table4.md)
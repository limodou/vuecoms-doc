# 修改记录

## V3.4
* Grid theme 新增 mini 风格，去掉表头的灰色背景，同时增加 zebra 参数，用控制是否显示表格行斑马线，缺省为 true
* 优化 Toast 点击关闭时，会调用两次 onClose 的 BUG
* 在 Vue.prototype 上增加 $Dialog 和 $ConfirmDialog 方法，用于打开对话框，详情参见对话框说明
* Build 字段定义中的 type 可以直接传 component 对象，不用声明成全局的了。例如：

    ```
    import MyList form '@/src/components/MyList'

    ...
    fields: [
      {name: 'mylist', type: MyList}
    ]
    ```


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
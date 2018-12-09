# 表格说明

## 基本用法

vuecoms表格是集：表格、查询条件、分页等控件于一体的控件，你可以根据需要进行设置，以决定某些特性是否启用。

```
<Grid :data="data" :value="value"></Grid>
```

表格的主要参数都放在 data(Object)中，value(Array)为对应的双向绑定的数据。

## data参数说明

| 属性参数名 | 说明 | 类型 | 缺省值 |
|----------|-----|------|------|
| columns | 表头定义，详情请参见《表头列定义说明》 | Array | 必填 |
| rowHeight | 行高 | Number | 34 |
| minColWidth | 最小列宽度 | Number | 5 |
| defaultColWidth | 缺省列宽度 | Number | 100 |
| nowrap | 是否自动换行 | Boolean | false |
| width | 表格宽度。如果为 'auto' 时， 会根据父元素宽度自动调整 | Number\|'auto'| 'auto' |
| height | 表格高度。如果为 'auto' 时，会根据内容高度自动调整 | Number\|'auto' | 'auto' |
| theme | 显示样式。目前支持： 'default', 'simple'。simple 则没有列的竖线。 | Enum | 'default' |
| checkCol | 是否显示checkbox列 | Boolean | false |
| checkColWidth | checkbox列的宽度 | Number | 30 |
| checkColTitle | checkbox列的标题 | String | '' |
| indexCol | 是否显示序号列 | Boolean | false |
| indexColWidth | 序号列的宽度 | Number | 40 |
| indexColTitle | 序号列的标题 | String | '#' |
| cellTitle | 是否将单元格内容作为title属性，这样当鼠标放上时，可以显示Tip | Boolean | true |
| idField | 表格数据名唯一字段的字段名。用于定位某一行，比如删除 | String | 'id' |
| multiSelect | 是否多选 | Boolean | false |
| clickSelect | 点击时是否选中。如果为 false ，则只能通过checkbox来选中，所以要把 checkCol 设为 true | Boolean | false |
| resizable | 是否可以设置表头列的宽度 | Boolean | true |
| draggable | 是否支持行拖拽 | Boolean | false |
| loadingText | 装入数据中显示的文本,支持HTML格式。 | String | '&lt;i class="icon-loading ivu-icon ivu-icon-load-c"&gt;&lt;/i&gt; Loading...' |
| url | 从后台装入数据的地址 | String | '' |
| autoLoad | 是否自动从后台装入数据 | Boolean | true |
| parseUrl | 是否自动从浏览器的URL上分析查询条件的参数 | Boolean | true |
| buttons | 表格头的按钮定义。将显示在表格左上角。具体定义格式详情参见下面的 按钮定义 | Array | [] |
| rightButtons | 表格头的按钮定义。将显示在表格右上角。具体定义格式详情参见下面的 按钮定义 | Array | [] |
| bottomButtons | 表格底的按钮定义。将显示在表格左下角。具体定义格式详情参见下面的 按钮定义 | Array | [] |
| combineCols | 单元格合并设置。它是一个二维数组。每一个数组定义了连续的表格列，当上下列的值相同时，会将上下单元格合并成一个。 | Array | [] |
| noData | 无数据时缺省显示的文本 | String | 暂无数据 |
| editMode | 编辑表格的模式。目前只支持行编辑，可以设为 'row' | String | '' |
| actionColumn | 当处于行编辑状态时，指定某列显示编辑按钮，如 "编辑", "删除" | String | '' |
| deleteRowConfirm | 当删除时，并且使用内置的删除处理时，是否在删除时要确认 | Boolean | true |
| query | 查询参数，详情参见 查询说明 | Object | {} |
| pagination | 分页展示切换 | Boolean | false |
| prev | 上一页按钮显示的文本 | String | '上一页' |
| next | 下一页按钮显示的文本 | String | '下一页' |
| first | 首页按钮显示的文本。为空时不显示 | String | '' |
| last | 最后一页按钮显示的文本。为空时不显示 | String | '' |
| start | 起始序号 | Number | 1 |
| total | 总条数。它将在后台加载数据后，由应用来根据具体值进行调整 | Number | 0 |
| pageSizeOpts | 每页显示条数设置 | Array | [10, 20, 30, 40, 50] |
| pageSize | 每页显示条数 | Number | 10 |
| tree | 是否按treegrid来显示 | Boolean | false |
| defaultExpanded | 缺省是否展表父结点 | Boolean | false |
| parentField | 父结点字段名 | String | 'parent' |
| treeField | 显示树型结构的列名 | String | '' |
| isParentField | 用于标识当前列是否为父结点的字段名 | String | '_isParent' |
| expandField | 用于标识父结点展开状态的字段名 | String | '_expand' |
| openedIcon | 父结点打开时显示的图标类名  | String | 'ivu-icon ivu-icon-arrow-down-b' |
| closedIcon | 父结点关闭时显示的图标类名  | String | 'ivu-icon ivu-icon-arrow-right-b' |
| indentWidth | 子结点缩近的宽度 | Number | 20 |
| iconWidth | 折叠图标的宽度 | Number | 14 |


| 回调函数名 | 说明 |
|----------|-----|
| onLoadData | 装入数据回调函数，将传入 function (url, param, callback)，当树型结构时，会传入parent字段 |
| onSelect | 在选择行前执行，返回为True，则允许选中 |
| onDeselect | 在取消选择行前执行，返回为True，则允许取消选中 |
| onCheckable | 是否显示checkbox |
| onSaveRow | 保存行时调用 function (row, callback)。当执行完毕时，需调用 callback ，格式为 callback(flag, data)。其中 flag 为 'ok' 表示成功，则data为最后的数据。 'error' 表示有错误， 则data为出错信息。 |
| onSaveCol | 保存单元格时调用 function (row, callback)。当执行完毕时，需调用 callback ，格式为 callback(flag, data)。其中 flag 为 'ok' 表示成功，则data为最后的数据。 'error' 表示有错误， 则data为出错信息。 |
| onDeleteRow | 删除行的确认 function (row, callback), callback(flag, data) |
| onRowEditRender | 行编辑对应的编辑按钮列的自定义渲染回调函数 function (h, row) h 为create函数，row为数据行。当返回 null 时，使用缺省的渲染函数，需要自行渲染时，应返回一个render调用结果，如： render('div', '本行不可编辑') |

## 表头列定义说明

| 属性参数名 | 说明 | 数据类型 | 缺省值 |
|----------|-----|------|------|
| name | 列名。不可为空。 | String |  |
| width | 列宽度。当不提供时，表示自适应宽度。 | Number | 0 |
| sortable | 是否显示排度指示。为 true 时，在字段标题右侧会显示一个图标，可以用来控制排序值。 | Boolean | false |
| align | 列的文字对齐。可选值为: 'left', 'right', 'center' | Enum | 'center' |
| hidden | 是否隐藏。可以控制某列是否显示。 | Boolean | false |
| fixed | 浮动位置。可选值为: 'left', 'right', '' | Enum | '' |
| html | 是否对内容进行转义。render不受此控制。format的结果受控制。 | Boolean | True |
| resizable | 是否可以拖拽改变宽度。| Boolean | true |
| editor | 如果可以编辑，则为编辑器的定义对象。它符合Build的字段定义规则。详情参见 Build的字段定义说明。| Object | {} |
| render | 自定义渲染函数。形式为 render: function(h, param)，其中h 为createElement函数。param为上下文参数，其值为：<br/>{<br/>value: 单元格的值<br/>column: 单元格定义对象<br/>row: 单元格所在行的数据对象<br/>}| Function | null |
| format | 自定义格式化函数。与render不同。render可以生成新的控件，而format只输出HTML。所以只要需要对单元格内容做文本加工的，可以使用format，如：加颜色，链接之类的。调用形式为：format: function(value, column, row)| Function | null |

## 表格按钮定义说明

表格按钮的定义和Build中的按钮定义是相同的。它是一个二维数组。每一个子数组通过 ButtonGroup 组织在一起。

每个按钮的定义说明如下：

| 属性参数名 | 说明 | 数据类型 | 缺省值 |
|----------|-----|------|------|
| label | 按钮标签 | String | 必填 |
| type | 按钮样式，可以选择：'primary', 'ghost', 'dashed', 'text', 'info', 'success', 'warning', 'error'| Enum | '' |
| shape | 形状。可以选择：'' 或 'circle' | Enum | '' |
| size | 大小。可以选择：'large', '', 'small' | Enum | '' |
| disabled | 是否禁用。 | Boolean | false |
| long | 是否长按钮 | Boolean | false |
| loading | 是否显示装入效果，为装入效果时禁用。 | Boolean | false |
| icon | 图标类型名。使用iview的图标名称 | String | '' |
| onClick | 点击时的回调函数。函数原型为 onClick: function (target, data), 其中target为表格控件本身。data为表格底层的store对象。 | Function | null |

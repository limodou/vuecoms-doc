# 基本用法

生成一个Box组件。

<div id="ex-box-01">
  <Box title="这是标题">
    <p>这是内容</p>
    <p>这是内容</p>
    <p>这是内容</p>
    <p>这是内容</p>
  </Box>
</div>

## slots 说明

| slot 名称 | 说明 |
|----------|-----|
| tools | 在Box标题处放置工具控件 |
| 缺省 | Box体放置内容 |
| footer | 在Box下面放置工具 |

## 参数说明

| 属性 | 说明 | 缺省值 |
|-----|-----|-------|
| title | 标题 | '' |
| withBorder | 是否显示border | true |
| headerClass | 表头样式，可以选 primary, success, warning, danger, default, info | '' |
| collapse | 是否可以收缩 | true |
| reomve | 是否可以删除 | false | 

<script>
var ex_box_01 = new Vue({
  el: '#ex-box-01'
})
</script>

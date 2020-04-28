# 基本用法

生成一个Box组件。

<div id="ex-box-01">
  <Box title="这是标题" :removable="removable"
    :theme="theme"
    :height="height"
    :with-border="border" :header-class="headerClass">
    <p>这是内容</p>
    <p>这是内容</p>
    <p>这是内容</p>
    <p>这是内容</p>
    <p>这是内容</p>
    <p>这是内容</p>
    <p>这是内容</p>
    <p>这是内容</p>
    <p>这是内容</p>
    <p>这是内容</p>
    <p>这是内容</p>
    <p>这是内容</p>
    <p>这是内容</p>
    <p>这是内容</p>
    <p>这是内容</p>
    <p>这是内容</p>
    <p>这是内容</p>
    <div slot="footer">
      <i-button type="primary" @click="handleBorder">{{displayBorder}}</i-button>
      <Dropdown style="margin-left: 20px" @on-click="handleClass">
        <i-button type="primary">
            切换Box的标题边框的样式
            <Icon type="arrow-down-b"></Icon>
        </i-button>
        <dropdown-menu slot="list">
            <dropdown-item name="primary">primary</dropdown-item>
            <dropdown-item name="success">success</dropdown-item>
            <dropdown-item name="warning">warning</dropdown-item>
            <dropdown-item name="danger">danger</dropdown-item>
            <dropdown-item name="default">default</dropdown-item>
            <dropdown-item name="info">info</dropdown-item>
        </dropdown-menu>
      </Dropdown>
      <i-button type="primary" @click="handleTheme">切换样式</i-button>
    </div>
  </Box>
</div>

## slots 说明

| slot 名称 | 说明 |
|----------|-----|
| tools | 在Box标题处放置工具控件 |
| 缺省 | Box体放置内容 |
| title | 标题区 |
| footer | 在Box下面放置工具 |

## 参数说明

| 属性 | 说明 | 缺省值 |
|-----|-----|-------|
| icon | 图标 | '' |
| title | 标题 | '' |
| withBorder | 是否显示表头border | true |
| withFooterBorder | 是否显示表尾border | true |
| headerClass | 表头样式，可以选 primary, success, warning, danger, default, info | '' |
| collapse | 是否显示收缩工具 | true |
| removable | 是否显示删除工具 | false | 
| headerAlign | 标题区对齐方式 | 'left' |
| theme | Box风格。可以支持 'card' 风格, simple | '' |
| height | Body的高度。可以是数值或字符串，如使用 'calc(100% - 51px)' | '' |

<script>
var ex_box_01 = new Vue({
  el: '#ex-box-01',
  data: {
    border: true,
    displayBorder: '隐藏表头线',
    headerClass: '',
    removable: true,
    theme: 'default',
    height: 300
  },
  methods: {
    handleBorder: function() {
      this.border = !this.border
      if (this.border) {
        this.displayBorder = '隐藏表头线'
      } else {
        this.displayBorder = '显示表头线'
      }
    },
    handleClass: function (name) {
      this.headerClass = name
    },
    handleTheme: function () {
      if (this.theme === 'default') this.theme = 'card'
      if (this.theme === 'card') this.theme = 'simple'
      else this.theme = 'default'
    }
  }
})
</script>

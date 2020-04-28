# 卡片列表

为了处理通用的卡片式列表结构，vuecoms提供了 CardList 组件，它可以提供自定义卡片列表的处理，如查询条件，分页，数据处理等，但是具体的展示交给用户自定义实现。

以下为一个示例：

```
<div id="ex-table-01">
  <card-list :config="table" v-if="enable" ref="cardlist">
    <template slot-scope="data">
      <card v-for="item of data.data" style="margin-bottom: 10px">
        <card-item :data="item"></card-item>
      </card>
    </template>
  </card-list>
  <Buttons :buttons="buttons"></Buttons>
</div>
```

其中 `<card-list>` 实现了整体的结构定义，它需要 `config` 用于定义参数信息。其中，用户需要定义

```
<template slot-scope="data">
  <card v-for="item of data.data" style="margin-bottom: 10px">
    <card-item :data="item"></card-item>
  </card>
</template>
```

用于实现自定义的卡片展示。这里使用了 `slot-scope` 的功能（目前vuecoms还不支持Vue 2.6.X）。数据将通过 `slot-scope` 来获取，这里为 `data` ，然后用户就可以通过 `data.data` 来获取底层的数据列表。

<div id="ex-table-01">
  <card-list :config="table" v-if="enable" ref="cardlist">
    <template slot-scope="data">
      <card v-for="item of data.data" style="margin-bottom: 10px">
        <card-item :data="item"></card-item>
      </card>
    </template>
  </card-list>
  <Buttons :buttons="buttons"></Buttons>
</div>

<script>
Vue.component('card-item', {
  template: ['<div>',
    '<div style="font-size: 20px">{{data.id}}-{{data.title}}</div>',
    '<div style="font-size: 9px">',
    '<Tag color="error" type="border">{{data.status}}</Tag>',
    ' <span>{{data.author}}</span>',
    ' <span>{{data.publishDate}}</span>',
    '</div>',
    '</div>'].join(''), 
  props: ['data']
})
var no = 1
var ex_table_01 = new Vue({
  el: '#ex-table-01',
  data: function () {
    var self = this
    var table = {
      pagination: true,
      scroll: false,
      onLoadData: function(param, callback) {
        setTimeout(function(){
          console.log(param)
          var data = []
          for(var i=0, len=10; i<len; i++) {
            data.push({id:(param.page - 1) * 10 + i + 1, title: '测试文章', status: 'top', author: '人民网', publishDate: '18分钟前'})
          }
          callback(data, {total: 100})
        }, 1000)
      },
      query: {
        fields: [
          {name: 'title'}
        ],
        layout: [
          ['title']
        ]
      }
    }
    var buttons = [
      [{label: '切换无限滚动', type: 'primary', onClick: function(target, data){
        self.$refs.cardlist.store.scroll = !self.$refs.cardlist.store.scroll
      }},
      ],
      [{label: '清除数据', type: 'primary', onClick: function(target, data){
        self.$refs.cardlist.store.data = []
      }},
      ]
    ]
    return {table:table, buttons: buttons, enable: true}
  }
})
</script>

-----

CardList 需要一个 `config` 的参数，它是一个对象，可选用的值为:

## config参数说明

| 属性参数名 | 说明 | 类型 | 缺省值 |
|----------|-----|------|------|
| query | 查询条件，同Grid的格式 | Object | null |
| pagination | 分页展示切换 | Boolean | false |
| prev | 上一页按钮显示的文本 | String | '上一页' |
| next | 下一页按钮显示的文本 | String | '下一页' |
| first | 首页按钮显示的文本。为空时不显示 | String | '' |
| last | 最后一页按钮显示的文本。为空时不显示 | String | '' |
| start | 起始序号 | Number | 1 |
| total | 总条数。它将在后台加载数据后，由应用来根据具体值进行调整 | Number | 0 |
| pageSizeOpts | 每页显示条数设置 | Array | [10, 20, 30, 40, 50] |
| pageSize | 每页显示条数 | Number | 10 |
| data | 底层数据 | Array | [] |
| autoLoad | 是否自动装入数据。为true时，会自动确发onLoadData回调。 | Boolean | true |
| onLoadData | 向后台发起请求的回调。与Grid类似，参数不同。格式为 onLoadData(param, callback) | Function | null |
| bottomButtons | 分页组件中的底部按钮 | Array | [] |
| nodata | 无数据时的信息，可以是HTML代码 | String | '暂无数据' |
| scroll | 是否无限滚动。如果为true，则分页组件将不显示，当滚动到底部时，会自动调用onLoadData方法。 | Boolean | false |


## CardList 方法说明

| 方法名 | 说明 | 返回值 |
|----------|-----|------|
| addRows | 向CardList中追加数据。如果为无限滚动，则原数据不会清除。如果为分页模式，则会进行替换 | 添加后的数据 |
| loadData | 装入数据，可用于刷新 | Promise |


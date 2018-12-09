# 布局展示

<div id="ex-grid-01">
  <Row>
    <i-col span="4">
      <reportlist :layout.sync="layout"></reportlist>
    </i-col>
    <i-col span="20">
      <grid-layout
            ref="gridlayout"
            :layout.sync="newlayout"
            :col-num="6"
            :row-height="150"
            :is-draggable="true"
            :is-resizable="true"
            :is-mirrored="false"
            :margin="[10, 10]"
            :use-css-transforms="true"
            :vertical-compact="true"
    >
        <!-- <component v-for="item in layout" :is="item.type" :item="item"></component> -->
        <grid-item :ref="'gi_'+item.i" v-for="item in newlayout" :key="item.i" :x="item.x" :y="item.y" :w="item.w" :h="item.h" :i="item.i" @resized="resizedEvent">
          <component :ref="item.i" :is="item.type" :item="item" :on-load-data="onLoadData"></component>
        </grid-item>
      </grid-layout>
    </i-col>
  </Row>
</div>
<style>
.vue-grid-item {
  border: solid gray 1px;
}
.ivu-card-head p {
  margin-bottom: 0px;
}
</style>
<script>
Vue.component('reportlist', {
  template: [
    '<Card title="可用报表" icon="ios-stats" :padding="0">',
      '<CellGroup @on-click="handleClick">',
          '<Cell v-for="item in layout" :name="item.i" :title="item.title" :selected="item.visiable" >',
            '<Icon v-if="item.visiable" type="md-checkmark" slot="extra"/>',
          '</Cell>',
      '</CellGroup>',
    '</Card>'
  ].join(''),
  props: ['layout'],
  methods: {
    handleClick: function(name) {
      for(var i=0, len=this.layout.length; i<len; i++) {
        if(this.layout[i].i === name) {
          var visiable = this.layout[i].visiable || false
          this.$set(this.layout[i], 'visiable', !visiable)
          if (visiable){
            this.$set(this.layout[i], 'x', 0)
            this.$set(this.layout[i], 'y', 2)
          }
          break
        }
      }
    }
  }
})
Vue.component('chartunit', {
  template: '<chart ref="chart" :options="item.options"></chart>',
  props: ['item', 'onLoadData'],
  methods: {
    resize: function(ow, oh) {
      var rect = this.$parent.calcPosition(this.item.x, this.item.y, this.item.w, this.item.h)
      var w = ow || rect.width
      var h = oh || rect.height
      this.$refs.chart.resize({width: w, height: h})
    },
    load: function(){
      var self = this
      if (this.onLoadData) {
        var callback = function (data) {
          self.$refs.chart.hideLoading()
          if (data) {
            self.$refs.chart.mergeOptions(data)
          }
        }
        this.$refs.chart.showLoading()
        this.onLoadData(this.item, callback)
      }
    }
  },
  mounted: function() {
    this.$nextTick(function(){
      this.resize()
      this.load()
    })
  }
})
Vue.component('sheetunit', {
  template: '<grid ref="grid" :data="item.options" style="margin:10px;"></grid>',
  props: ['item', 'onLoadData'],
  methods: {
    resize: function(ow, oh) {
      var rect = this.$parent.calcPosition(this.item.x, this.item.y, this.item.w, this.item.h)
      var w = (ow || rect.width) - 20
      var h = (oh || rect.height) + this.item.dh
      this.$refs.grid.resize(w, h)
    },
    load: function(){
      var self = this
      if (this.onLoadData) {
        var callback = function (data) {
          if (data)
            self.$refs.grid.mergeOptions(data)
        }
        this.onLoadData(this.item, callback)
      }
    }
  },
  mounted: function() {
    this.$nextTick(function(){
      this.resize()
      this.load()
    })
  }
})
var chart1 = {type: 'chartunit', title: '报表1', x:0, y:0, w:6, h:2, i:"chart1", 
  sqlname:'test1',
  options:{
    title: {
        text: '图表示例',
        left: 'left'
    },
    tooltip : {
        trigger: 'axis',
        axisPointer: {
            type: 'cross',
            label: {
                backgroundColor: '#6a7985'
            }
        }
    },
    toolbox: {
        feature: {
            saveAsImage: {},
            magicType: {
                type: ['line', 'bar']
            }
        },
    },
    dataset: {
      source: [],
      demensions: ['name', {name: 'value1', displayName: '工作量'}]
    },
    xAxis: {
        type: 'category',
    },
    yAxis: {},
    series: [
      {type: 'line'}
    ]
  }
}
var chart2 = {type: 'chartunit', title: '报表2', x:0, y:0, w:6, h:2, i:"chart2", 
  sqlname:'test2',
  options:{
    legend: {},
    title: {
        text: '图表示例',
        left: 'left'
    },
    tooltip : {
        trigger: 'axis',
        axisPointer: {
            type: 'cross',
            label: {
                backgroundColor: '#6a7985'
            }
        }
    },
    toolbox: {
        feature: {
            saveAsImage: {},
            magicType: {
                type: ['line', 'bar']
            }
        },
    },
    dataset: {
      source: [],
      demensions: ['name', {name: 'value1', displayName: '工作量1'}, {name: 'value2', displayName: '工作量2'}]
    },
    xAxis: {
        type: 'category',
    },
    yAxis: {},
    series: [
      {type: 'bar'},
      {type: 'bar'}
    ]
  }
}
var sheet1 = {
  type: 'sheetunit', title: '报表3', x:0, y:0, w:6, h:2, dh:-90, i:"chart3", 
    sqlname:'test3',
    options: {
      nowrap: true,
      indexCol: true,
      theme: 'default',
      pagination: true,
      columns: [
        {name:'name1', title:'Name1', width:100, fixed: 'left'},
        {name:'name2', title:'Name2', width: 100},
        {name:'name3', title:'Name3', width:100},
        {name:'name4', title:'Name4', align:'center', width:400, html: false},
        {name:'name5', title:'Name5', width:400},
        {name:'name6', title:'Name6', fixed: 'right'}
      ],
      param: {
        str1: "Hello World!!!",
        tree: '1'
      },
      onLoadData: function (url, param, callback) {
        self.param = param
        var data = []
        var b = (param.page - 1) * param.pageSize
        for (var i = 0; i < param.pageSize; i++) {
          var row = {id: b + i + 1, title: 'P' + param.page + '-Title-' + (i + 1)}
          for (var j = 1; j < 7; j++) {
            row['name' + j] = 'P' + param.page + '-Name-' + (i + 1) + '-' + j
          }
          data.push(row)
        }
        setTimeout( function () {
          callback(data, {total:100})
          }, 0)
      },
    }
}
var testLayout = [
    chart1,
    chart2,
    sheet1,
	];
	var ex_grid_01 = new Vue({
    el: '#ex-grid-01',
    data: {
      layout: testLayout,
    },
    computed: {
      newlayout: function (){
        var list = []
        for(var i=0, len=this.layout.length; i<len; i++) {
          this.$set(this.layout[i], 'visiable', this.layout[i].visiable || false)
          if (this.layout[i].visiable) {
            list.push(this.layout[i])
          }
        }
        return list
      }
    },
    methods: {
      resizedEvent: function(i, newH, newW, newHPx, newWPx) {
        this.$refs[i][0].resize(newWPx, newHPx)
      },
      onLoadData: function(item, callback) {
        console.log(item.sqlname)
        if (item.type === 'chartunit'){
          setTimeout(function(){
            var result = {
              dataset: {
                source: [
                  {name: 'Mon', value1: 820, value2: 240},
                  {name: 'Tue', value1: 932, value2: 340},
                  {name: 'Wed', value1: 901, value2: 640},
                  {name: 'Thu', value1: 934, value2: 840},
                  {name: 'Fri', value1: 1290, value2: 340},
                  {name: 'Sat', value1: 1330, value2: 540},
                  {name: 'Sun', value1: 1320, value2: 720},
                ]
              } 
            }
            callback(result)
          }, 2000)
        }
        else {
          callback()
        }
      }
    }
	});
  </script>
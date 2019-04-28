# 通用表格

根据配置生成通用表格查询. 例如配置中可以带sqlname，对应后台的通用查询。这里通用查询的配置信息都是数据，不能包含
js代码。

<div id="ex-table-06">
  <common-grid :data="table" ref="grid" :on-load-data="onLoadData"
    :on-load-choices="onLoadChoices"
    @input="handleInput"></common-grid>
  <div>{{result}}</div>
</div>

<script>
var loaddata = function(data, param, condition, callback) {
  console.log(data.sqlname, param, condition)
  setTimeout(function() {
    var d = []
    d.push({id:1, name1:'A1', name2:'01', name3:'C1', name4:'D1', name5:'E1'})
    d.push({id:2, name1:'A2', name2:'01', name3:'C1', name4:'D1', name5:'E1'})
    d.push({id:3, name1:'A1', name2:'02', name3:'C1', name4:'D1', name5:'E1'})
    d.push({id:4, name1:'A2', name2:'02', name3:'C1', name4:'D1', name5:'E2'})
    d.push({id:5, name1:'A2', name2:'02', name3:'C1', name4:'D1', name5:'E2'})
    d.push({id:6, name1:'A1', name2:'01', name3:'C1', name4:'D1', name5:'E3'})
    callback(d, {total: 100})
  }, 0)
}
var loadchoices = function(choices, callback) {
  console.log(choices)
  callback({name2: [{label: 'Value1', value: '01'}, {label: 'Value2', value: '02'}]})
}
var isEmpty = function (v) {
  if (Array.isArray(v)) {
    return v.length === 0
  } else if (v instanceof Object) {
    for (var c in v) {
      return false
    }
    return true
  }
  return !v
}
var value = {
  str: 'abc',
  str2: 'cde',
  str3: '3',
  str4: '4',
  int: 12,
  int2: '12',
  range: ['123', '456'],
  range2: ['123', '456'],
}
var config = {
  str: '>',
  str2: 'like%',
  str3: '%like%',
  str4: '%like',
  int: '=',
  int2: '<',
  range: 'between',
  range2: 'in',
}
var convert_value = function (value) {
  if (typeof value === 'int')
    return value + ''
  else if (typeof value === 'string')
    return "'" + value + "'"
  return value
}
var make_field_condition = function(field, op, value) {
  if (isEmpty(value) || !op) return ''
  switch (op) {
    case 'between':
      return field + ' between ' + convert_value(value[0]) + ' and ' + convert_value(value[1])
    case 'daterange':
      if (value[0] && value[1]) 
        return field + ' >= ' + convert_value(value[0]) + ' and ' + field + ' <= ' + convert_value(value[1])
      else if (value[0])
        return field + ' >= ' + convert_value(value[0])
      else
        return field + ' <= ' + convert_value(value[1])
    case 'in':
      return field + ' in (' + ')'
    case 'like%':
      return field + ' like ' + convert_value(value+ '%')
    case '%like%':
      return field + ' like ' + convert_value('%' + value + '%')
    case '%like':
      return field + ' like ' + convert_value('%' + value)
    default:
      return field + ' ' + op + ' ' + convert_value(value)
  }
}
var makecondition = function (c, value) {
  var condition = []
  for(var x in value) {
    var v = make_field_condition(x, c[x], value[x])
    if (v)
      condition.push(v)
  }
  return condition.join(' and ')
}
console.log(makecondition(config, value))
Vue.component('common-grid', {
  template: '<Grid v-if="data" :data="data" :choices="choices" @input="handleInput"></Grid>',
  props: ['data', 'onLoadData', 'onLoadChoices'],
  data: function(){
    return {choices: {}}
  },
  watch: {
    data: function(v){
      var self = this
      if (v) {
        v.onLoadData = this.loadData
        if (v.download) {
          v.buttons = [
            [
              {label: '下载', type:'primary', onClick: function(target, data){
                  self.$Message.info('下载')
                }
              },
            ]
          ]
        }
        if (!isEmpty(v.choices)) {
          this.loadChoices(v.choices)
        }
      }
    }
  },
  methods: {
    loadData: function (url, param, callback) {
      if (this.data.sqlname) {
        var config = this.get_condition_config()
        var condition = makecondition(config, param)
        if (this.onLoadData) {
          this.onLoadData(this.data, param, condition, callback)
        }
      }
    },
    loadChoices: function(choices) {
      var self = this
      var keys = []
      for(var k in choices) {
        keys.push(k)
      }
      var callback = function(result){
        for(var k in (result || {})) {
          self.$set(self.choices, k, result[k])
        }
      }
      if (this.onLoadChoices) {
        this.onLoadChoices(keys, callback)
      }
    },
    handleInput: function(v) {
      this.$emit('input', v)
    },
    get_condition_config: function() {
      var fields = this.data.query.fields, field, config = {}
      for (var i=0, len=fields.length; i<len; i++) {
        field = fields[i]
        if (field.sqlop) config[field.name] = field.sqlop
      }
      return config
    }
  }
})
var table = {
  nowrap: true,
  indexCol: true,
  theme: 'default',
  pagination: true,
  download: true,
  columns: [
    {name:'name1', title:'Name1', width:100, fixed: 'left', sortable: true},
    {name:'name2', title:'Name2', width: 100, sortable: true, editor: {type: 'select'}},
    {name:'name3', title:'Name3', width:100, format: '<a href="/test/${row.name2}">${row.name1}</a>', sortable: true},
    {name:'name4', title:'Name4', align:'left', width:400},
    {name:'name5', title:'Name5'},
  ],
  sqlname: 'testsql',
  choices: {
    name2: []
  },
  query: {
    fields: [
      {name: 'name1', label: 'Name1', sqlop: '%like%'},
      {
        name: 'datepickerrange',
        type: 'datepickerrange',
        label: '日期范围',
        sqlop: 'daterange',
        options: {
          placeholderBegin: '开始时间',
          placeholderEnd: '结束时间'
        }
      },

    ],
    layout: [
      [{name: 'name1', colspan: 12}], 
      ['datepickerrange']
    ]
  }
}
var ex_table_06 = new Vue({
  el: '#ex-table-06',
  data: function () {
    return {table:null, result:{}, onLoadData: loaddata, onLoadChoices: loadchoices}
  },
  mounted: function () {
    var self = this
    setTimeout(function(){
      self.table = table
    }, 500)
  },
  methods:{
    handleInput: function(v){
      this.result = v
    }
  }
})
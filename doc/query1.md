# 基本查询

<div id="ex-query-01">
  <Query ref="query" :fields="fields" :layout="layout"
    :choices="choices"
    :default-value="defaultValue" :show-line="2"
    :value="value"
    :label-width="labelWidth"
    :show-selected="true"
    @on-query-change="handleQueryChange"></Query>
    {{value}}
</div>
<script>
var ex_query_01 = new Vue({
  el: '#ex-query-01',
  methods: {
        changed: function (data) {
          console.log(data);
          return true;
        },
        submit: function(data){
          console.log("submit event => ",data);
          return true
        },
        handleQueryChange: function (change) {
          console.log(change)
        }
  },
  data: function () {
        var self = this
        return {
          labelWidth: 120,
          fields: [
                    {name: "str1", type: "Year", label: "字符串1", static: true, placeholder: "请输入字符串1",
                      options: {
                        icon: "ios-clock-outline",
                      },
                      on: {
                        'on-click': function () {
                          self.$Message.info('on-click')
                        }
                      }
                    },
                    {name: "str2", type: "string", label: "字符串2", placeholder: "请输入字符串2"},
                    {
                      name: "select1",
                      type: "select",
                      label: "选择1",
                      placeholder: "这是一个下拉单选项",
                      options: {
                        filterable: true
                      }
                    },
                    {
                      name: "select2",
                      type: "select",
                      label: "选择2",
                      multiple: true,
                      placeholder: "这是一个下拉多选项"
                    },
                    {
                      name: "datepicker",
                      type: "date",
                      label: "日期",
                      placeholder: "日期单选",
                      options: {
                        // type: 'year',
                        confirm: true,
                        size: "default",
                        placement: "bottom",//top top-start top-end bottom bottom-start bottom-end left left-start left-end right right-start right-end (default bottom-start)
                        disabledDate: function (date) {
                          return date && date.valueOf() < Date.now() - 86400000;
                        },
                        shortcuts: [
                          {
                            text: 'Today',
                            value: function() {
                              return new Date();
                            },
                            onClick: function(picker){
                              this.$Message.info('Click today');
                            }
                          },
                          {
                            text: 'Yesterday',
                            value: function(){
                              var date = new Date();
                              date.setTime(date.getTime() - 3600 * 1000 * 24);
                              return date;
                            },
                            onClick: function(picker){
                              this.$Message.info('Click yesterday');
                            }
                          },
                          {
                            text: 'One week',
                            value: function(){
                              var date = new Date();
                              date.setTime(date.getTime() - 3600 * 1000 * 24 * 7);
                              return date;
                            },
                            onClick: function(picker){
                              this.$Message.info('Click a week ago');
                            }
                          }
                        ]
                      }
                    },
                    {
                      name: 'datepickerrange',
                      type: 'datepickerrange',
                      label: '日期范围',
                      options: {
                        placeholderBegin: '开始时间',
                        placeholderEnd: '结束时间'
                      }
                    },
                    {
                      name: "radio",
                      type: "radio",
                      label: "单选框",
                      options: {
                        choices: [
                          {value: "1", label: "备选项1"}, 
                          {value: "2", label: "备选项2", disabled: true}, 
                          {value: "3", label: "备选项3"}
                        ]
                      }
                    },
                    {
                      name: "checkbox",
                      type: "checkboxgroup",
                      label: "多选框",
                      options: {
                        choices: [
                          {value: "1", label: "备选项1"}, 
                          {value: "2", label: "备选项2", disabled: true}, 
                          {value: "3", label: "备选项3"}
                        ]
                      }
                    },
                    {
                      name: "select_remote",
                      type: "select",
                      label: "远程选择",
                      options: {
                        remote: true,
                        filterable: true,
                        remoteMethod: function(term, callback) {
                          setTimeout(function(){
                            console.log('select_remote remoteMethod', term)
                            callback([{label: 'Test A', value: 'A'}, {label: 'Test B', value: 'B'}])
                          }, 500)
                        },
                        remoteSelected: function (v, callback) {
                          if (v)
                          setTimeout(function(){
                            console.log('select_remote remoteSelected', v)
                            callback([{label: 'Test A', value: 'A'}])
                          }, 20)
                        }
                      }
                    },
                    {
                      name: "select_remote2",
                      type: "select",
                      label: "远程选择2",
                      multiple: true,
                      options: {
                        remote: true,
                        filterable: true,
                        remoteMethod: function(term, callback) {
                          setTimeout(function(){
                            console.log('select_remote2 remoteMethod', term)
                            callback([{label: 'Test A', value: 'A'}, {label: 'Test B', value: 'B'}])
                          }, 500)
                        },
                        remoteSelected: function (v, callback) {
                          if (v)
                          setTimeout(function(){
                            console.log('select_remote2 remoteSelected', v)
                            callback([{label: 'Test A', value: 'A'}])
                          }, 20)
                        }
                      }
                    },
                  ],
                  layout: [
                    ['str1', 'str2'],
                    ['select1', 'select2'],
                    ['datepicker', 'datepickerrange'],
                    ['radio', 'checkbox'],
                    ['select_remote', 'select_remote2']
                  ],
                  defaultValue: {
                    select1: 'city_003',
                    select2: ["city_001"],
                    str1: "Hello World!!!",
                    checkbox: ["1","2"],
                    radio: "1",
                  },
                  value: {
                    datepickerrange: ['2018-04-20', '2018-04-22'],
                    select_remote: 'A',
                    select_remote2: ['A']
                  },
                  choices: {
                    select1: [{label: "西雅图", value: "city_001"}, {label: "旧金山", value: "city_002"}, {
                      label: "洛杉矶",
                      value: "city_003"
                    }],
                    select2: [
                      {label: "西雅图", value: "city_001"},
                      {label: "旧金山", value: "city_002"},
                      {label: "洛杉矶", value: "city_003"}
                    ],
                  },
                  buttons: {
                    align: "center",//按钮左中右 start center end 默认 end
                    submit: {
                      label: "点此查询",
                    },
                    clear: {
                      label: "点此清除",
                      show:false
                    }
                  }
        }
      },
})
</script>

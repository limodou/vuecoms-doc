# 基本查询

<div id="ex-query-01">
  <query-form ref="query" :fields="fields" :layout="layout"
    :choices="choices"
    :value="value" :buttons="buttons" :show-line="1"
    :label-width="labelWidth"
    :show-selected="true"
    @on-query-change="handleQueryChange"></query-form>
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
        handleQueryChange (change) {
          console.log(change)
        }
  },
  data: function () {
        return {
          labelWidth: 120,
          fields: [
                    {name: "str1", type: "str", label: "字符串1", placeholder: "请输入字符串1"},
                    {name: "str2", type: "str", label: "字符串2", placeholder: "请输入字符串2"},
                    {
                      name: "select1",
                      type: "iselect",
                      label: "选择1",
                      multiple: false,
                      clearable: true,
                      disabled: false,
                      filterable: true,
                      size: "default", // small default large
                      placeholder: "这是一个下拉单选项"
                    },
                    {
                      name: "select2",
                      type: "iselect",
                      label: "选择2",
                      multiple: true,
                      disabled: false,
                      filterable: true,
                      placeholder: "这是一个下拉多选项"
                    },
                    {
                      name: "datepicker",
                      type: "daterange",
                      label: "日期",
                      placeholder: "日期单选",
                      range: true,
                      format: "yyyy#MM#dd",
                      confirm: true,
                      size: "default",
                      disabled: false,
                      placement: "bottom",//top top-start top-end bottom bottom-start bottom-end left left-start left-end right right-start right-end (default bottom-start)
                      readonly: false,
                      editable: false,
                      clearable: false,
                      transfer: false,
                      options: {
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
                      name: "radio",
                      type: "radio",
                      label: "单选框",
                      rType: "button", // button or null
                      disabled: false,
                      size: "small",
                      vertical: true,
                      choices: [{label: "1", name: "备选项1"}, {label: "2", name: "备选项2", disabled: true}, {
                        label: "3",
                        name: "备选项3"
                      }]
                    },
                    {
                      name: "checkbox",
                      type: "checkbox",
                      label: "多选框",
                      size: "large", //small default large
                      disabled: false,
                      choices: [{label: "1", name: "备选项1"}, {label: "2", name: "备选项2"}, {
                        label: "3",
                        name: "备选项3",
                        disabled: true
                      }]
                    },
                  ],
                  layout: [
                    ['str1', 'str2'],
                    ['select1', 'select2'],
                    ["datepicker"],
                    ['radio', 'checkbox']
                  ],
                  value: {
                    select1: 'city_003',
                    select2: ["city_001"],
                    str1: "Hello World!!!",
                    checkbox: ["1","2"],
                    radio: "1",
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

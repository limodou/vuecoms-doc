# Select Rich

<div id="ex-build-01">
  <build ref="build" :data="data" :value="value" :errors="errors" ></build>
  <div>{{value}}</div>
</div>
<script>
var ex_build_01 = new Vue({
  el: '#ex-build-01',
  data: function () {
    var self = this
    var data = [
      {
        name: 'basic',
        title: '基本信息',
        labelWidth: 150,
        staticSuffix: '_static',
        fields: [
          {name: 'str1', label: '字符串1', placeholder: '请输入...', help: '帮助信息',
            info: 'info信息', required: true, rule: {type: 'email'}, onChange: function(value, data){
              self.$set(data, 'str2', value)
            }},
          {name: 'str2', label: '静态字符串2', static: true, required: true, format: function(v){
            return '<a href="#">' + v + '</a>'
          }},
          {name: 'select1', label: '选择1', type: 'select', static: false, 
            required: true, rule: {type: 'object'}, options: {
              rich: true,
              filterable: true, 
              remote: true, 
              clearable: true,
              remoteMethod: function(query, callback){
                setTimeout(function(){
                  if (query === 'a')
                    callback([{value:'A', label:'Test A', text: 'A'}, {value:'B', label:'Test B', text: 'B'}, {value:'C', label:'Test C', text: 'C'}])
                  else
                    callback([{value:'D', label:'Test D', text: 'D'}, {value:'E', label:'Test E', text: 'E'}, {value:'F', label:'Test F', text: 'F'}])
                  }, 1000)
              }
            },
            onChange: function (value, alldata) {
              self.$set(alldata, 'str', value.label || '')
            }
          },
          {name: 'str', label: '选择2', type: 'str', static: true},
        ],
        layout: [
          ['str1', 'str2'],
          ['select1', 'str'],
        ],
        boxOptions: {widthBorder: false, headerClass: 'primary'},
        buttons: {
          items: [
            [{label: '查看结果', type:'primary', onClick: function(target, data){
                console.log(target, data)
              }
            }],
            [{label: '设置Select1为D', type:'info', onClick: function(target, data){
                self.$set(self.value, 'select1', {value: 'D', label: '选项二'})
              }
            }],
            [{label: '设置Str1为Y', type:'info', onClick: function(target, data){
                self.$set(self.value, 'str1', 'Y')
              }
            }],
          ],
        }
      },
    ]
    return {
            data:data,
            value: {select1: '', str: ''},
            errors: {},
          }
  },
  methods: {
    save: function(error) {
      if (error) {
        this.$Message.error(error)
      } else {
        this.$Message.info('saved')
      }
    }
  },
  mounted: function () {
    var self = this
    setTimeout(function () {
      self.value = {select1: {value: 'A', label: '选项一'},
        str: 'aaaa',
        str1: 'X'
      }
    }, 1000)
  }
})
</script>

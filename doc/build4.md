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
          {name: 'select1', label: '选择1', type: 'select', static: false, 
            required: true, rule: {type: 'object'}, options: {
              rich: true,
              clearable: true,
              choices: [
                {label:'选项一', value: 'A'},
                {label:'选项二', value: 'B'},
              ] 
            },
            onChange: function (value, alldata) {
              console.log('aaaaaa', value)
              self.$set(alldata, 'str', value.value || '')
            }
          },
          {name: 'str', label: '选择2', type: 'str', static: true},
        ],
        layout: [
          ['select1', 'str'],
        ],
        boxOptions: {widthBorder: false, headerClass: 'primary'},
        buttons: {
          items: [
            [{label: '查看结果', type:'primary', onClick: function(target, data){
                console.log(target, data)
              }
            }],
            [{label: '设置为B', type:'info', onClick: function(target, data){
                self.$set(self.value, 'select1', {value: 'B', label: '选项二'})
              }
            }],
          ],
        }
      },
    ]
    return {
            data:data,
            value: {},
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
      self.value = {
              select1: {value: 'A', label: '选项一'},
              str: ''
            }
    }, 1000)
  }
})
</script>

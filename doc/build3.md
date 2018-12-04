# 数据联动

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
          {name: 'select1', label: '选择1', type: 'select', required: true, options: {clearable: true,
              choices: [
                {label:'选项一', value: 'A'},
                {label:'选项二', value: 'B'},
              ] 
            },
            onChange: function (value, alldata) {
              self.$set(alldata, 'select2', value)
            }
          },
          {name: 'select2', label: '选择2', type: 'str', static: true},
        ],
        layout: [
          ['select1', 'select2'],
        ],
        boxOptions: {widthBorder: false, headerClass: 'primary'},
        buttons: {
          items: [
            [{label: '查看结果', type:'primary', onClick: function(target, data){
                console.log(target, data)
              }
            }],
          ],
        }
      },
    ]
    return {
            data:data,
            value: {
              select1: '',
              select2: ''
            },
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
    /* var self = this
    setTimeout(function () {
      var c = [
        {label:'选项一', value: 'A'},
        {label:'选项二', value: 'B'},
        {label:'选项三', value: 'C'}
      ]
      self.$set(self.choices, 'select1', c)
    }, 1000) */
  }
})
</script>

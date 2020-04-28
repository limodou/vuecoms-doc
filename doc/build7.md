# 数据字典使用

<div id="ex-build-01">
  <build ref="build" :data="data" :value="value" :choices="choices" :datadict="datadict" :errors="errors" ></build>
  <div>{{value}}</div>
</div>
<script>
var ex_build_01 = new Vue({
  el: '#ex-build-01',
  data: function () {
    var self = this
    var datadict = {
      'select1': {name: 'select1', label: '选择1', type: 'select', required: true, options: {clearable: true}},
      'select2': {name: 'select2', label: '选择2', type: 'select', static: true}
    }
    var data = [
      {
        name: 'basic',
        title: '基本信息',
        fields: [
          {ref: 'select1',
            onChange: function (value, alldata) {
              self.$set(alldata, 'select2', value)
            }
          },
          'select2',
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
            datadict: datadict,
            value: {
              select1: '',
              select2: ''
            },
            choices: {
              select1: [],
              select2: [],
              select3: []
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
    var self = this
    setTimeout(function () {
      self.$set(self.value, 'select1', 'A')
      self.$set(self.value, 'select3', ['A', 'B'])
    }, 50)
    setTimeout(function () {
      var c = [
        {label:'选项一', value: 'A'},
        {label:'选项二', value: 'B'},
        {label:'选项三', value: 'C'}
      ]
      self.$refs.build.setChoices('select1', c)
      self.$refs.build.setChoices('select2', c)
      self.$refs.build.setChoices('select3', c)
      // self.$set(self.choices, 'select1', c)
      // self.$set(self.choices, 'select2', c)
      // self.$set(self.choices, 'select3', c)
    }, 1000)
  }
})
</script>

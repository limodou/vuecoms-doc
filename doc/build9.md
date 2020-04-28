# 输入输出转换

在Build中，存在象select需要Object的数据类型的情况，及对于某些自定义的组件，有自已的数据校式要求。但是这些数据结构
可能与后台返回的数据并不一致，因此需要进行转换。转换的地方为：从后台读取数据，赋值给Build的value时，和向后台提交数据时。可能会存在转入和转出两个方向。

根据目前存在的转换场景，整理出两种模式：

* mapConvert 
  
    它的作用是将分散的字段聚合成一个对象和反向将一个对象拆分为不同的属性，适用于select需要Object对象的场景。

    定义时可以：

    ```
    fields: [
      {name: 'field', mapConvert: {label: 'field_a', value: 'field_b'}, keep: false}
    ]
    ```

    上面的配置表示 field 这个字段的值，是由 {label: value:} 组成的，其中label由field_a提供，value由field_b提供。

* inputConvert/outputConvert

    转入及转出处理。它是一个函数，函数签名为：function(value, field, data)，其中value是对应的字段值，field
    是对应的字段定义，data是整个build的数据。转入使用inputConvert来处理，用于输入数据转为字段控件所需要的数据
    结构。转出使用outputConvert来处理，用于输出到外部如保存使用，将组件返回的数据转为你希望的格式。

对于从后台读取数据后，传入Build之前，我们可以这样处理：

```
let res = await service() // 读后台数据
v = this.$refs.build.processInputConvert(res) // 对数据进行转换，根据上面两种模式
this.value = v // 赋值给value
```

对于将数据转为外部数据，我们可以这样处理：

```
self.$refs.build.getData()
```

这里还有一个要注意的，如果Build的字段上定义了 `keep: false` 时，这个数据将不被输出，可以忽略一些只用于前端的一些字段。

getData() 主要的作用：

1. 数据清理，去除 _ 开头的值, _static 结尾的值
2. 数据转换，即处理上述两种场景的转出数据
3. 清理field.keep为false的值

<div id="ex-build-01">
  <build ref="build" :data="data" :value="value" :buttons="buttons"></build>
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
        fields: [
          {name: 'select', label: '远程选择', type: 'select', 
          mapConvert: {label: 'select_name', 'value': 'select_id'},
          keep: false,
          options: {
            clearable: true,
            filterable: true, 
            remote: true, 
            rich: true, 
            remoteMethod: function(query, callback){
              setTimeout(function(){
                if (query === 'a')
                  callback([{value:'A', label:'Test A', text: 'A'}, {value:'B', label:'Test B', text: 'B'}, {value:'C', label:'Test C', text: 'C'}])
                else
                  callback([{value:'D', label:'Test D', text: 'D'}, {value:'E', label:'Test E', text: 'E'}, {value:'F', label:'Test F', text: 'F'}])
                }, 1000)
              }
            }
          },
          {name: 'select_id', hidden: true},
          {name: 'select_name', hidden: true},
          {name: 'keyword', label: '关键字', inputConvert: function (v, field, data) {
            return v.split(',').join('@')
          },
          outputConvert: function (v, field, data){
            return v.split('@').join(',')
          }}
        ],
        layout: [
          ['select'],
          ['keyword']
        ]
      },
    ]
    return {
            data:data,
            value: {},
            buttons: [
              [{label: '查看结果', type:'primary', onClick: function(target, data){
                  console.log(self.$refs.build.getData())
                  self.$Message.info(JSON.stringify(self.$refs.build.getData()))
                }
              }],
            ]
          }
  },
  mounted () {
    setTimeout(()=>{
      let v = {select_id: 'A', 'select_name': 'Test A', keyword: 'a,b,c'}
      v = this.$refs.build.processInputConvert(v)
      this.value = v
    }, 50)
  }
})
</script>

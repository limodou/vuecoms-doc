# 表格拖动

<div id="ex-table-03">
  <Grid ref="table" :data="table" @on-drag="handleDrag"></Grid>
</div>
<script>
var ex_table_03 = new Vue({
  el: '#ex-table-03',
  data: function () {
    var self = this
    var table = {
      nowrap: true,
      draggable: true,
      clickSelect: true,
      columns: [
        {name:'name1', title:'Name1', width:200,
          render: function(h, param){
            return h('RadioGroup', {
                props: {
                  value: param.value
                },
                on: {
                  'input': function (value) {
                    param.row[param.column.name] = value
                  }
                }
              }, [
                h('Radio', {
                  props: {
                    label: 'yes'
                  }
                }, '同意'),
                h('Radio', {
                  props: {
                    label: 'no'
                  }
                }, '不同意')
              ])
          }
        },
        {name:'name2', title:'Name2', width: 200,
          render: function(h, param){
            return h('Input', {
                props: {
                  value: param.value
                },
                on: {
                'input': function (value) {
                    param.row[param.column.name] = value
                  }
                }
              })
          }
        },
        {name:'name3', title:'Name3', width:200},
        {name:'name4', title:'Name4', width:200},
      ],
      buttons: [
        [{label: '查看结果', type:'primary', onClick: function(){
            console.table(self.$refs.table.store.states.data)
          }}]
      ],
      data: []
    }
    table.data.push({id:1, name1:'A1', name2:'B1', name3:'C1', name4:'D1'})
    table.data.push({id:2, name1:'A2', name2:'B2', name3:'C2', name4:'D2'})
    table.data.push({id:3, name1:'A3', name2:'B3', name3:'C3', name4:'D3'})
    table.data.push({id:4, name1:'A4', name2:'B4', name3:'C4', name4:'D4'})
    table.data.push({id:5, name1:'A5', name2:'B5', name3:'C5', name4:'D5'})
    table.data.push({id:6, name1:'A6', name2:'B6', name3:'C6', name4:'D6'})
    return {table:table}
  },
  methods: {
    handleDrag: function (v) {
      console.log(v)
    }
  }
})
</script>

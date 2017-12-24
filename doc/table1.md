# 简单表格

<div id="ex-table-01">
  <Grid :data="table"></Grid>
</div>

<script>
var ex_table_01 = new Vue({
  el: '#ex-table-01',
  data: function () {
    var table = {
      nowrap: true,
      columns: [
        {name:'name1', title:'Name1', width:200, fixed: 'left'},
        {name:'name2', title:'Name2', width: 200, fixed: 'left'},
        {name:'name3', title:'Name3', width:200},
        {name:'name4', title:'Name4', align:'center', width:200},
        {name:'name5', title:'Name5', width:200},
        {name:'name6', title:'Name6', width:200}
      ],
      data: [],
      combineCols:['name1', 'name2', 'name3', 'name4']
    }

    table.data.push({id:1, name1:'A1', name2:'B1', name3:'C1', name4:'D1', name5:'E1', name6:'F1'})
    table.data.push({id:2, name1:'A1', name2:'B1', name3:'C1', name4:'D1', name5:'E1', name6:'F1'})
    table.data.push({id:3, name1:'A1', name2:'B2', name3:'C1', name4:'D1', name5:'E1', name6:'F1'})
    table.data.push({id:4, name1:'A2', name2:'B2', name3:'C1', name4:'D1', name5:'E1', name6:'F1'})
    table.data.push({id:5, name1:'A2', name2:'B2', name3:'C1', name4:'D1', name5:'E1', name6:'F1'})
    table.data.push({id:6, name1:'A1', name2:'B1', name3:'C1', name4:'D1', name5:'E1', name6:'F1'})

    return {table:table}
  }
})
</script>

# 树控件

<div id="ex-tree-01" style="width: 200px; overflow: auto; border: 1px solid gray;">
  <Tree :data="data"></Tree>
</div>
<script>
var ex_tree_01 = new Vue({
  el: '#ex-tree-01',
  data: function () {
    var data = [
      {
        title: 'parent 1',
        expand: true,
        children: [
          {
            title: 'parent 1-1 - long- long- long- long- long- long',
            expand: true,
            children: [
              {
                title: 'leaf 1-1-1- long- long- long- long- long- long'
              },
              {
                title: 'leaf 1-1-2'
              }
            ]
          },
          {
            title: 'parent 1-2',
            expand: true,
            children: [
              {
                title: 'leaf 1-2-1'
              },
              {
                title: 'leaf 1-2-1'
              }
            ]
          }
        ]
      }
    ]
    return {data:data}
  }
})
</script>

# 树选择控件

<div id="ex-tree-01">
  <tree-select :choices="choices" v-model="value" clearable filterable></tree-select>
  <pre>{{value}}</pre>
</div>
<script>
var ex_tree_01 = new Vue({
  el: '#ex-tree-01',
  data: function () {
    var data = [
      {
        id: 'a',
        title: 'parent 1',
        expand: true,
        children: [
          {
            id: 'a1',
            title: 'parent 1-1 - long- long- long- long- long- long',
            expand: true,
            children: [
              {
                id: 'a1-1',
                title: 'leaf 1-1-1- long- long- long- long- long- long'
              },
              {
                id: 'a1-2',
                title: 'leaf 1-1-2'
              }
            ]
          },
          {
            id: 'a2',
            title: 'parent 1-2',
            expand: true,
            children: [
              {
                id: 'a2-1',
                title: 'leaf 1-2-1'
              },
              {
                id: 'a2-2',
                title: 'leaf 1-2-1'
              }
            ]
          }
        ]
      }
    ]
    return {choices: data, value:''}
  }
})
</script>

<div id="ex-tree-02">
  <tree-select :choices="choices" v-model="value" multiple filterable></tree-select>
  <pre>{{value}}</pre>
</div>
<script>
var ex_tree_02 = new Vue({
  el: '#ex-tree-02',
  data: function () {
    var data = [
      {
        id: 'a',
        title: 'parent 1',
        expand: true,
        children: [
          {
            id: 'a1',
            title: 'parent 1-1 - long- long- long- long- long- long',
            expand: true,
            children: [
              {
                id: 'a1-1',
                title: 'leaf 1-1-1- long- long- long- long- long- long'
              },
              {
                id: 'a1-2',
                title: 'leaf 1-1-2'
              }
            ]
          },
          {
            id: 'a2',
            title: 'parent 1-2',
            expand: true,
            children: [
              {
                id: 'a2-1',
                title: 'leaf 1-2-1'
              },
              {
                id: 'a2-2',
                title: 'leaf 1-2-1'
              }
            ]
          }
        ]
      }
    ]
    return {choices: data, value:''}
  }
})
</script>

### 远程单选示例

<div id="ex-tree-03">
  <tree-select :choices="choices" v-model="value" remote
    :remote-load-data="handleRemoteLoadData">
  </tree-select>
  <pre>{{value}}</pre>
</div>
<script>
var ex_tree_03 = new Vue({
  el: '#ex-tree-03',
  data: function () {
    var data = [
    ]
    return {choices: data, value:''}
  },
  mounted: function () {
    var self = this
    setTimeout(function () {
      self.choices = [{
        id: 'parent',
        title: 'parent',
        loading: false,
        children: []
        }]
      }, 3000)
  },
  methods: {
    handleRemoteLoadData: function (item, callback) {
      if (item) {
        callback([
          {
              title: 'children1',
              id: 'children1'
          },
          {
              id: 'children2',
              title: 'children2'
          }
        ])
      }
    }
  }
})
</script>

### 远程多选示例

<div id="ex-tree-04">
  <tree-select :choices="choices" v-model="value" multiple filterable remote
    :remote-query="handleRemoteQuery" :remote-load-data="handleRemoteLoadData">
  </tree-select>
  <pre>{{value}}</pre>
</div>
<script>
var ex_tree_04 = new Vue({
  el: '#ex-tree-04',
  data: function () {
    var data = [
    ]
    return {choices: data, value:''}
  },
  mounted: function () {
    var self = this
    setTimeout(function () {
      self.choices = [{
        id: 'parent',
        title: 'parent',
        loading: false,
        children: []
        }]
      }, 3000)
  },
  methods: {
    handleRemoteQuery: function (query, callback) {
      var self = this
      setTimeout( function () {
        var data = [
          {
            id: 'a',
            title: 'parent a',
            expand: true,
            children: [
              {
                id: 'a1',
                title: 'parent a1',
                expand: true,
                children: [
                  {
                    id: 'a1-1',
                    title: 'a1-1'
                  },
                  {
                    id: 'a1-2',
                    title: 'a1-2'
                  }
                ]
              },
              {
                id: 'a2',
                title: 'parent a2',
                expand: true,
                children: [
                  {
                    id: 'a2-1',
                    title: 'a2-1'
                  },
                  {
                    id: 'a2-2',
                    title: 'a2-2'
                  }
                ]
              }
            ]
          }
        ]
        callback(data)
      }, 300)
    },
    handleRemoteLoadData: function (item, callback) {
      if (item) {
        callback([
          {
              title: 'children1',
              id: 'children1'
          },
          {
              id: 'children2',
              title: 'children2'
          }
        ])
      }
    }
  }
})
</script>

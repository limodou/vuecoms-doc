# 对话框

<div id="ex-editor-01">
  <i-button type="primary" @click="open=true">Show Modal</i-button>
  <Modal v-model="open"
    title="对话框标题"
    width="1024"
  >
    <build ref="build" :data="data" :value="value"></build>
  </Model>
</div>
<script>
var choices = [
  {label:'选项一', value: 'A'},
  {label:'选项二', value: 'B'},
  ]
var ex_editor_01 = new Vue({
  el: '#ex-editor-01',
  data: function () {
    var data = [
      {
        name: 'basic1',
        title: '基本信息1',
        fields: [
          {name: 'str1', label: '字符串1', placeholder: '请输入...', help: '帮助信息',
            info: 'info信息', required: true, rule: {type: 'email'}},
          {name: 'str2', label: '静态字符串2', static: true, required: true, convert: function(v){
            return '<a href="#">' + v + '</a>'
            }
          },
          {name: 'select1', label: '选择', type: 'select', required: true, options: {choices: choices}},
          {name: 'select2', label: '选择', type: 'select', static: true, options: {choices: choices}},
          {name: 'rich1', label: '富文本', type: 'tinymce', options: { options: {height: 200}}},
          {name: 'tree1', label: '树选择', required: true, type: 'treeselect', multiple: true, options: {choices:
            [ {
                  id: 'fruits',
                  title: 'Fruits',
                  children: [ {
                    id: 'apple',
                    title: 'Apple',
                  }, {
                    id: 'grapes',
                    title: 'Grapes',
                  }, {
                    id: 'pear',
                    title: 'Pear',
                  }, {
                    id: 'strawberry',
                    title: 'Strawberry',
                  }, {
                    id: 'watermelon',
                    title: 'Watermelon',
                  } ],
                }, {
                  id: 'vegetables',
                  title: 'Vegetables',
                  children: [ {
                    id: 'corn',
                    title: 'Corn',
                  }, {
                    id: 'carrot',
                    title: 'Carrot',
                  }, {
                    id: 'eggplant',
                    title: 'Eggplant',
                  }, {
                    id: 'tomato',
                    title: 'Tomato',
                  } ],
                } ]
              }
          },
        ],
        layout: [
          ['str1', 'str2'],
          ['select1', 'select2'],
          ['tree1'],
          ['rich1']
        ],
        boxComponent: ''
      }
    ]
    return {
      open: false,
      data: data,
      value: {
        select1: 'B',
        select2: 'A',
        select3: ['A', 'B'],
        select4: ['A', 'B'],
      }
    }
  }
})
</script>

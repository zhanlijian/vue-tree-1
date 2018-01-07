<template>
  <ul class="vue-tree-list">
    <tree-item
      v-for="(item, idx) in treeData"
      :ids="ids"
      :ids-with-parent="idsWithParent"
      :model="item"
      :options="termOptions"
      :depth="0"
      :state="state"
      :key="idx"
      @handle="handle"
    />
  </ul>
</template>

<script>
  import Item from './Item.vue'
  import '../vue-tree.css'

  export default {
    components: { 'tree-item': Item },

    model: { // 组件封装id绑定
      prop: 'ids',
      event: 'change'
    },

    props: {
      treeData: {
        type: Array,
        default: function () {
          return []
        }
      },
      options: {
        type: Object,
        default: function () {
          return {}
        }
      },
      ids: {
        type: Array,
        default: function () {
          return []
        }
      }
    },

    data () {
      return {
        defaultOptions: {
          label: 'label',
          checkbox: true,  //显示复选框
          checkedOpen: true, // 已选择是否展开
          folderBold: true, //目录加粗
          idsWithParent: true, // 复选是否包含目录
          depthOpen: 0, //初始展开层级
          openIcon: 'fa fa-angle-right', //展开
          closeIcon: 'fa fa-angle-down', // 关闭
          halfCheckedIcon: 'fa fa-minus-square-o fa-fw', //部分选择
          checkedIcon: 'fa fa-check-square-o fa-fw', // 全选
          uncheckedIcon: 'fa fa-square-o fa-fw' // 不选
        },
        termOptions: {},
        idsWithParent: [],
        state: 0
      }
    },

    created () {
      this.initOptions()
      this.idsWithParent = this.ids.slice(0)
    },

    watch: {
      options: {
        handler: function (val) {
          this.initOptions()
        },
        deep:  true
      }
    },

    methods: {
      handle (item) {
        this.$emit('handle', item)
      },
      initOptions () {
        this.termOptions = Object.assign({}, this.defaultOptions, this.options)
        this.idsWithParent = this.ids.slice(0)
      }
    }
  }
</script>

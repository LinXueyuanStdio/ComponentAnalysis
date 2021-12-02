<template>
  <div id="Tree">
    <vue-ztree
      :treeNodes="treeNodes"
      @onClick="handleTreeClick"
      :check="check"
      @onCheck="handleCheck"
    /><p>sdfdsfsdfd</p>
  </div>
</template>

<script>
import VueZtree from 'ztree-for-vue'
export default {
  name: 'Tree',
  components: {
    VueZtree
  },
  props: {
    treeNodes: {
      type: Array,
      default: function () {
        return [
          {
            name: '父节点1',
            id: 0,
            children: [
              {
                name: '子节点1',
                id: 1
              },
              {
                name: '子节点2',
                id: 2,
                children: [
                  {
                    name: '子节点3',
                    id: 3
                  }
                ]
              }
            ]
          }
        ]
      }
    },
    check: {
      type: Object,
      default: function () {
        return {
          enable: true
        }
      }
    }
  },
  watch: {
    data (newData, oldData) {
      console.log(newData, oldData)
    }
  },
  methods: {
    handleTreeClick (event, treeId, treeNode) {
      console.log(event, treeId, treeNode)
    },
    handleCheck (event, treeId, treeNode) {
      console.log(event, treeId, treeNode)
    }
  }
}
</script>

<style lang="scss" scoped>
@media only screen and (max-width: 1125px) {
  #info,
  #mode {
    display: none !important;
  }
}
#Tree {
  font-size: 18px;
  color: #034c6a !important;
}
</style>

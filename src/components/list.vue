<template>
  <div id="Tree">
    <vue-ztree
      :treeNodes="treeNodes"
      @onClick="handleTreeClick"
      :check="check"
      @onCheck="handleCheck"
    />
  </div>
</template>

<script>
import axios from 'axios'
import VueZtree from 'ztree-for-vue'
export default {
  name: 'Tree',
  data () {
    return {
      treeNodes: axios.get('../data/tree.json').then(resp => resp.data),
      check: {
        enable: true
      }
    }
  },
  components: {
    VueZtree
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

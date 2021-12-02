<template>
  <div class="gContainer">
    <gSearch @getData="update" />
    <Tree :treeNodes="treeNodes" :check="check" />
  </div>
</template>

<script>
import gSearch from '@/components/gSearch.vue'
import Tree from '@/components/Tree.vue'
import axios from 'axios'
export default {
  components: {
    gSearch,
    Tree
  },
  data () {
    return {
      treeNodes: [],
      check: {
        enable: true
      }
    }
  },
  methods: {
    // 视图更新
    update (json) {
      console.log('update')
      console.log(json)
      this.d3jsonParser(json)
    },
    // 解析json数据，主要负责数据的去重、标准化
    d3jsonParser (json) {
      axios.get('../data/tree.json').then((resp) => {
        console.log(resp.data.data)
        this.treeNodes = resp.data.data
        this.check = {
          enable: true
        }
      })
    }
  }
}
</script>

<style lang="scss" scoped>
.gContainer {
  position: relative;
  border: 2px #000 solid;
  background-color: #9dadc1;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  overflow: hidden;
}
</style>

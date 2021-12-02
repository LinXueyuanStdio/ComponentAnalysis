<template>
  <div class="gContainer">
    <gSearch @getData="update" />
    <Tree :data="data" />
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
      data: []
    }
  },
  methods: {
    // 视图更新
    update (json) {
      console.log('update')
      console.log(json)
      axios.get('../data/tree.json').then((resp) => {
        console.log(resp.data.data)
        this.data = resp.data.data
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

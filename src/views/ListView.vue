<template>
  <div class="gContainer">
    <gSearch @getData="update" />
    <!-- <threeGraph /> -->
    <List :data="data" />
  </div>
</template>

<script>
// @ is an alias to /src
import gSearch from '@/components/gSearch.vue'
import List from '@/components/List.vue'

export default {
  name: 'ListView',
  components: {
    gSearch,
    List
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
      this.data = [
        {
          name: 'qwed',
          size: 'sdfsd',
          type: 'sdfas',
          pkgName: 'asdf',
          extInfo: 'sdf',
          level: 'asdf',
          useCount: 1,
          useCountImmediate: 1,
          libCount: 1,
          sizeValue: 1
        }
      ]
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
  justify-content: center;
  align-items: center;
  overflow: hidden;
}
</style>

<template>
  <div class="gContainer">
    <gSearch @getData="update" />
    <!-- <threeGraph /> -->
    <Graph3d
      :data="data"
      :names="names"
      :labels="labels"
      :linkTypes="linkTypes"
    />
  </div>
</template>

<script>
// @ is an alias to /src
import gSearch from '@/components/gSearch.vue'
import Graph3d from '@/components/Graph3d.vue'

export default {
  name: 'Graph3dView',
  components: {
    gSearch,
    Graph3d
  },
  data () {
    return {
      // d3jsonParser()处理 json 后返回的结果
      data: {
        nodes: [],
        links: []
      },
      names: ['企业', '贸易类型', '地区', '国家'],
      labels: ['Enterprise', 'Type', 'Region', 'Country'],
      linkTypes: ['', 'type', 'locate', 'export']
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
      const nodes = []
      const links = [] // 存放节点和关系
      const nodeSet = [] // 存放去重后nodes的id
      for (const item of json) {
        for (const segment of item.p.segments) {
          // 重新更改data格式
          if (nodeSet.indexOf(segment.start.identity) === -1) {
            nodeSet.push(segment.start.identity)
            nodes.push({
              id: segment.start.identity,
              label: segment.start.labels[0],
              properties: segment.start.properties
            })
          }
          if (nodeSet.indexOf(segment.end.identity) === -1) {
            nodeSet.push(segment.end.identity)
            nodes.push({
              id: segment.end.identity,
              label: segment.end.labels[0],
              properties: segment.end.properties
            })
          }
          links.push({
            source: segment.relationship.start,
            target: segment.relationship.end,
            type: segment.relationship.type,
            properties: segment.relationship.properties
          })
        }
      }
      console.log(nodes)
      console.log(links)
      // this.links = links
      // this.nodes = nodes
      this.data = { nodes, links }
      // return { nodes, links }
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

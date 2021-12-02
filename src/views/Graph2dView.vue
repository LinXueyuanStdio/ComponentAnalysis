<template>
  <div class="gContainer">
    <!-- <Graph2d /> -->
    <gSearch @getData="update" />
    <Graph2d
      :data="data"
      :names="names"
      :labels="labels"
      :linkTypes="linkTypes"
    />
  </div>
</template>

<script>
import gSearch from '@/components/gSearch.vue'
import Graph2d from '@/components/Graph2d.vue'
export default {
  components: {
    gSearch,
    Graph2d
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
    buildItem (item) {
      console.log(item)
      return {
        id: item.identity,
        label: item.id ? item.id : '',
        properties: {
          groupId: item.id,
          fileSize: item.fileSize,
          versionCode: '版本号',
          versionName: '版本名称',
          detail: item.detail,
          totalSize: item.totalSize,
          name: item.name ? item.name : ''
        }
      }
    },
    parse (nodes, links, nodeSet, parentItem) {
      if (!parentItem) {
        return
      }
      const source = this.buildItem(parentItem)
      console.log(source)
      if (!parentItem.children) {
        return
      }
      for (const item of parentItem.children) {
        if (!item) {
          continue
        }
        const itemId = item.identity
        if (nodeSet.indexOf(itemId) === -1) {
          nodeSet.push(itemId)
          const target = this.buildItem(item)
          nodes.push(target)
          links.push({
            source: source,
            target: target,
            type: 'type',
            properties: {
              name: '类型'
            }
          })
          this.parse(nodes, links, nodeSet, item)
        }
      }
    },
    d3jsonParser (json) {
      const nodes = []
      const links = [] // 存放节点和关系
      const nodeSet = [] // 存放去重后nodes的id
      this.parse(nodes, links, nodeSet, json.data[0])
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
  align-items: center;
  justify-content: center;
  overflow: hidden;
}
</style>

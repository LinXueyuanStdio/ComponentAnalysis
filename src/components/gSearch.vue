<template>
  <div style="margin-top: 20px; width: 500px">
    <!-- <el-button style="margin-top: 15px;" @click="query">图数据切换，动态更新</el-button> -->
    <el-autocomplete
      style="width: 500px"
      class="inline-input"
      v-model="input"
      :fetch-suggestions="querySearch"
      placeholder="请输入内容"
      :trigger-on-focus="false"
      @select="handleSelect"
      clearable
    >
      <!-- <el-select
        v-model="mode"
        slot="prepend"
        placeholder="关键字查询"
      >
        <el-option label="关键字查询" value="1"></el-option>
        <el-option label="单实体查询" value="2"></el-option>
        <el-option label="关联查询" value="3"></el-option>
      </el-select> -->
      <el-button
        slot="append"
        type="success"
        icon="el-icon-search"
        @click="query"
        >搜索</el-button
      >
    </el-autocomplete>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'gSearch',
  // props: {
  //   isShowPrepend: {
  //     type: Boolean,
  //     default: true
  //   }
  // },
  data () {
    return {
      input: '',
      mode: '1',
      // 后台请求到的json数据
      data: [],
      results: []
    }
  },
  mounted () {
    this.query()
  },
  methods: {
    query () {
      axios.get('../data/tree.json').then((resp) => {
        this.data = resp.data
        this.$emit('getData', this.data)
      })
    },
    querySearch (queryString, cb) {
      var res = this.data
      var results = queryString
        ? res.filter(this.createFilter(queryString))
        : res
      // 调用 callback 返回建议列表的数据
      cb(results)
    },
    createFilter (queryString) {
      return (res) => {
        return (
          res.value.toLowerCase().indexOf(queryString.toLowerCase()) !== -1
        )
      }
    },
    handleSelect (item) {
      console.log(item)
    }
  }
}
</script>

<style lang='scss' scoped>
.el-select {
  width: 120px;
  // background-color: #fff;
}
.input-with-select .el-input-group__prepend {
  background-color: #6ecbf3;
}
</style>

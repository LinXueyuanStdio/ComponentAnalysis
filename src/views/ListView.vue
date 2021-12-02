<template>
  <div id="List">
    <el-table
      :data="tableData"
      height="550"
      border
      style="width: 100%"
    show-summary
       :summary-method="getSummaries"
      :default-sort="{ prop: 'sizeValue', order: 'descending' }"
    >
      <el-table-column prop="name" label="名称" sortable>
      </el-table-column>
      <el-table-column prop="size" label="大小" sortable>
      </el-table-column>
      <el-table-column prop="type" label="类型" sortable>
      </el-table-column>
      <el-table-column prop="pkgName" label="包名" sortable>
      </el-table-column>
      <el-table-column prop="extInfo" label="额外信息">
      </el-table-column>
      <el-table-column prop="level" label="等级" sortable>
      </el-table-column>
      <el-table-column prop="useCount" label="被使用次数" sortable>
      </el-table-column>
      <el-table-column
        prop="useCountImmediate"
        label="被直接依赖次数"
        sortable
        width="180"
      >
      </el-table-column>
      <el-table-column
        prop="libCount"
        label="包含依赖库个数"
        sortable
        width="180"
      >
      </el-table-column>
      <el-table-column prop="sizeValue" label="大小" sortable width="180">
      </el-table-column>
    </el-table>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'List',
  data () {
    return {
      tableData: []
    }
  },
  mounted () {
    axios.get('../data/list.json').then((resp) => {
      console.log(resp.data.data)
      this.tableData = resp.data.data
    })
  },
  methods: {
    getSummaries (param) {
      const { columns, data } = param
      const sums = []
      columns.forEach((column, index) => {
        if (index === 0) {
          sums[index] = '总结'
          return
        }
        const values = data.map(item => Number(item[column.property]))
        if (!values.every(value => isNaN(value))) {
          sums[index] = values.reduce((prev, curr) => {
            const value = Number(curr)
            if (!isNaN(value)) {
              return prev + curr
            } else {
              return prev
            }
          }, 0)
          sums[index] += ' '
        } else {
          sums[index] = 'N/A'
        }
      })

      return sums
    }
  }
}
</script>

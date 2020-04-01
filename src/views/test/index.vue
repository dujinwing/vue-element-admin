<template>
  <div class="app-container">
    <aside>
      点击下方按钮渲染测试表格
      <a href="https://github.com/kamranahmedse/driver.js" target="_blank">driver.js.</a>
    </aside>
    <el-button icon="el-icon-question" type="primary" @click.prevent.stop="add">
      新增数据
    </el-button>
    <el-button icon="el-icon-question" type="primary" @click.prevent.stop="del">
      删除数据
    </el-button>

    <el-table ref="multipleTable" :data="tableData" stripe style="width:100%" @selection-change="tableChange">
      <el-table-column
        type="selection"
        width="55"
      />
      <el-table-column prop="id" label="ID" />
      <el-table-column prop="name" label="姓名">
        <template slot-scope="scope">
          <el-popover trigger="hover" placement="top">
            <p>姓名: {{ scope.row.name }}</p>
            <p>结果: {{ scope.row.result }}</p>
            <div slot="reference" class="name-wrapper">
              <el-tag size="medium">{{ scope.row.name }}</el-tag>
            </div>
          </el-popover>
        </template>
      </el-table-column>
      <el-table-column :formatter="dateFormat" prop="date" label="创建日期" />
      <el-table-column label="操作">
        <template>
          <el-button type="text" @click="view">查看</el-button>
          <el-button type="text" @click="edit">编辑</el-button>
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>

<script>
import Driver from 'driver.js' // import driver.js
import 'driver.js/dist/driver.min.css' // import driver.js css
import axios from 'axios'

var index = 0
var selectedData = []

export default {

  name: 'Guide',
  data() {
    return {
      driver: null,
      tableData: []
    }
  },
  mounted() {
    this.driver = new Driver()
  },
  methods: {
    guide() {
      this.driver.start()
    },
    tableChange(rows) {
      selectedData = rows
    },
    dateFormat(row, column, value) {
      var date = new Date()
      return date.getFullYear() + '-' + (date.getMonth() + 1) + '-' + date.getDate()
    },
    add() {
      axios
        .get('http://127.0.0.1:8080/api-c/consumeByFeign?name=test&token=123')
        .then(response => {
          // 存储token
          const data = {
            id: index++,
            result: response.data,
            name: '请求' + index
          }
          this.tableData.push(data)
        })
    },
    edit() {
      console.log(selectedData)
    },
    del() {
      for (const i in selectedData) {
        console.log('index:' + i + ' data:' + selectedData[i].name)
      }
    },
    view() {

    }
  }
}
</script>

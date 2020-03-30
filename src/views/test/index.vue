<template>
  <div class="app-container">
    <aside>
      点击下方按钮渲染测试表格
      <a href="https://github.com/kamranahmedse/driver.js" target="_blank">driver.js.</a>
    </aside>
    <el-button icon="el-icon-question" type="primary" @click.prevent.stop="render">
      发送请求
    </el-button>

    <el-table :data="tableData" stripe style="width:100%">
      <el-table-column prop="id" label="ID" width="180" />
      <el-table-column prop="name" label="姓名" width="180">
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
      <el-table-column :formatter="dateFormat" prop="date" label="创建日期" width="180" />
    </el-table>
  </div>
</template>

<script>
import Driver from 'driver.js' // import driver.js
import 'driver.js/dist/driver.min.css' // import driver.js css
import axios from 'axios'

var index = 0

// axios拦截器
axios.interceptors.request.use(function(config) {
  let token = ''
  const accessToken = window.localStorage.getItem('accessToken')
  if (accessToken) {
    token = accessToken
  }
  config.headers.common['authorization'] = token

  return config
})
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
    dateFormat(row, column, value) {
      var date = new Date()
      return date.getFullYear() + '-' + (date.getMonth() + 1) + '-' + date.getDate()
    },
    render() {
      axios
        .get('http://127.0.0.1:8080/api-a/hello?name=test&token=123')
        .then(response => {
          // 存储token
          const data = {
            id: index++,
            result: response.data,
            name: '请求' + index
          }
          this.tableData.push(data)
        })
    }
  }
}
</script>

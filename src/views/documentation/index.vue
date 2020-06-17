<template>
  <div class="app-container">
    <!-- 添加可编辑表格 -->
    <h1>添加可编辑表格</h1>
    <el-button @click="addLine">添加</el-button>
    <el-table :data="list" element-loading-text="Loading" border>
      <el-table-column label="序号" type="index" width="100" />
      <el-table-column label="电话" align="center">
        <template slot-scope="scope">
          <el-input v-model="scope.row.tel" :disabled="!scope.row.disabled" />
        </template>
      </el-table-column>
      <el-table-column label="地址" align="center">
        <template slot-scope="scope">
          <el-input v-model="scope.row.address" :disabled="!scope.row.disabled" />
        </template>
      </el-table-column>
      <el-table-column label="操作" align="center">
        <template slot-scope="scope">
          <el-button v-if="scope.row.isAdd" @click="saveLine(scope)">保存</el-button>
        </template>
      </el-table-column>
    </el-table>
    <!-- 筛选表头的表格 -->
    <h1>筛选表头的表格</h1>
    <span @click='clickHeader'>筛选表头</span>
    <div v-if='isShow'>
      <el-checkbox-group v-model="checkeds">
        <el-checkbox v-for="(item,index) in items" :key="index" :label='item.value' @change="boxChange(item)"></el-checkbox>
      </el-checkbox-group>
    </div>
    <el-table :key="key" :data="tables" element-loading-text="Loading" border style="width: 100%">
      <el-table-column fixed prop="date" label="日期" width="150"></el-table-column>
      <el-table-column v-for="(item,index) in items" :key="index" :label="item.text" :prop="item.value" >
        <template slot-scope="scope">
          {{ scope.row[item.value]}}
        </template>
      </el-table-column>
      <el-table-column
      fixed="right"
      width="120"
      >
      <template slot="header">
        <span> 操作</span>
        <i class="el-icon-edit" ></i>
      </template>
      </el-table-column>
    </el-table>
  </div>
</template>
<script>
export default {
  name: 'Documentation',
  components: {

  },
  data() {
    return {
      list: [
        {
          tel: 13111111111,
          address: '天津市南开区',
          disabled: false
        }
      ],
      isShow: false,
      list1: [], // table表格的全部字段的数据
      items: [], // 动态表头存放的数组
      value1: [], // 选中的表头
      checkeds: [],
      tables: [ // 筛选表格内容
        {
          date: '2016-05-03',
          name: '王小虎',
          sex: '男',
          hubby: '唱歌',
          tel: '11111111111',
          province: '上海',
          city: '普陀区',
          address: '上海市普陀区金沙江路 1518 弄',
          zip: 200333
        }, {
          date: '2016-05-02',
          name: '王小虎',
          sex: '男',
          hubby: '唱歌',
          tel: '11111111111',
          province: '上海',
          city: '普陀区',
          address: '上海市普陀区金沙江路 1518 弄',
          zip: 200333
        }, {
          date: '2016-05-04',
          name: '王小虎',
          sex: '男',
          hubby: '唱歌',
          tel: '11111111111',
          province: '上海',
          city: '普陀区',
          address: '上海市普陀区金沙江路 1518 弄',
          zip: 200333
        }, {
          date: '2016-05-01',
          name: '王小虎',
          sex: '男',
          hubby: '唱歌',
          tel: '11111111111',
          province: '上海',
          city: '普陀区',
          address: '上海市普陀区金沙江路 1518 弄',
          zip: 200333
        }, {
          date: '2016-05-08',
          name: '王小虎',
          sex: '男',
          hubby: '唱歌',
          tel: '11111111111',
          province: '上海',
          city: '普陀区',
          address: '上海市普陀区金沙江路 1518 弄',
          zip: 200333
        }, {
          date: '2016-05-06',
          name: '王小虎',
          sex: '男',
          hubby: '唱歌',
          tel: '11111111111',
          province: '上海',
          city: '普陀区',
          address: '上海市普陀区金沙江路 1518 弄',
          zip: 200333
        }, {
          date: '2016-05-07',
          name: '王小虎',
          sex: '男',
          hubby: '唱歌',
          tel: '11111111111',
          province: '上海',
          city: '普陀区',
          address: '上海市普陀区金沙江路 1518 弄',
          zip: 200333

        }
      ],
      key: 1
    }
  },
  watch: {
    checkeds(valArr) {
      // console.log(valArr)
      this.formThead = this.items.filter(i => valArr.indexOf(i) >= 0)
      this.key = this.key + 1// 为了保证table 每次都会重渲 In order to ensure the table will be re-rendered each time
    }
  },
  mounted() {
    this.getTable()
  },
  methods: {
    addLine() {
      const newLine = {
        tel: this.tel,
        address: this.address,
        isAdd: true,
        disabled: true // 打开编辑状态
      }
      this.list.unshift(newLine) // 移到第一行
    },
    saveLine(scope) {
      console.log(scope)
    },
    getTable() {
      const arr = [
        {
          date: '2016-05-02',
          name: '王小虎',
          linkMan: '王小虎',
          address: '上海市普陀区金沙江路 1518 弄'
        }, {
          date: '2016-05-04',
          name: '王小虎',
          linkMan: '王小虎',
          address: '上海市普陀区金沙江路 1517 弄'
        }, {
          date: '2016-05-01',
          name: '王小虎',
          linkMan: '王小虎',
          address: '上海市普陀区金沙江路 1519 弄'
        }, {
          date: '2016-05-03',
          name: '王小虎',
          linkMan: '王小虎',
          address: '上海市普陀区金沙江路 1516 弄'
        }
      ]
      const arr1 = [
        { text: '姓名', value: 'name' },
        { text: '性别', value: 'sex' },
        { text: '爱好', value: 'hubby' },
        { text: '电话', value: 'tel' },
        { text: '地址', value: 'address' },
        { text: '省份', value: 'province' },
        { text: '城市', value: 'city' },
        { text: '邮编', value: 'zip' }
      ]
      this.items = arr1
      this.list1 = arr
    },
    clickHeader() {
      this.isShow = true
    },
    boxChange(data) {
      console.log(this.checkeds)
    }
  }
}
</script>

<style lang='scss' scoped>

</style>

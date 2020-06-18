<template>
  <div class="app-container">
    <aside>
      The guide page is useful for some people who entered the project for the
      first time. You can briefly introduce the features of the project. Demo is
      based on
      <a href="https://github.com/kamranahmedse/driver.js" target="_blank"
        >driver.js.</a
      >
    </aside>
    <el-button
      icon="el-icon-question"
      type="primary"
      @click.prevent.stop="guide"
    >
      Show Guide
    </el-button>
    <div>
      ***********************************************************************************
    </div>
    <div>{{ notice }}</div>
    <div v-for="(elem, i) in items" :key="i">
      <el-checkbox
        v-model="elem.parameters"
        @change="handleCheckAllChange(elem)"
        >全选</el-checkbox
      >
      <el-checkbox-group v-model="elem.parameters" @change="boxChange(elem)">
        <el-checkbox
          v-for="(item, index) in elem.typeData"
          :key="index"
          :label="item.val"
        />
      </el-checkbox-group>
    </div>
    <div>
      ***********************************************************************************
    </div>
    <el-calendar v-model='value'>
      <template slot="dateCell" slot-scope="{ date, data }">
        <p
          :class="
            solarDate2lunar(data.day).isFestval || solarDate2lunar(data.day).isLunarFestival ? 'normal active' : 'normal'
          "
          @click="clickEveryone(date)"
        >
          {{ solarDate2lunar(data.day).date }}
        </p>
      </template>
    </el-calendar>
  </div>
</template>

<script>
import Driver from 'driver.js' // import driver.js
import 'driver.js/dist/driver.min.css' // import driver.js css
import steps from './steps'
import Buss from '@/utils/bus'
import calendar from '@/utils/calendar.js'
export default {
  name: 'Guide',
  data() {
    return {
      driver: null,
      notice: '',
      items: [],
      isIndeterminate: true,
      checkAll: false,
      aaa: [], // 选中
      bbb: [], // 全部选项
      value: new Date()
    }
  },
  created() {
    Buss.$on('aaa', data => {
      // console.log(data)
      this.notice = data
      // console.log(this.notice)
    })
    this.$nextTick(() => {
      // 点击前一个月
      const prevBtn = document.querySelector(
        '.el-calendar__button-group .el-button-group>button:nth-child(1)'
      )
      prevBtn.addEventListener('click', () => {
        const yy = new Date(this.value).getFullYear()
        const mm = this.checkTime(new Date(this.value).getMonth() + 1)
        const dd = this.checkTime(new Date(this.value).getDate())
        const dates = yy + '-' + mm + '-' + dd
        console.log(dates)
      })
    })
    this.$nextTick(() => {
      // 点击前一个月
      const prevBtn = document.querySelector(
        '.el-calendar__button-group .el-button-group>button:nth-child(2)'
      )
      prevBtn.addEventListener('click', () => {
        const yy = new Date().getFullYear()
        const mm = this.checkTime(new Date().getMonth() + 1)
        const dd = this.checkTime(new Date().getDate())
        const dates = yy + '-' + mm + '-' + dd
        console.log(dates)
      })
    })

    this.$nextTick(() => {
      // 点击后一个月
      const prevBtn = document.querySelector(
        '.el-calendar__button-group .el-button-group>button:last-child'
      )
      prevBtn.addEventListener('click', () => {
        const yy = new Date(this.value).getFullYear()
        const mm = this.checkTime(new Date(this.value).getMonth() + 1)
        const dd = this.checkTime(new Date(this.value).getDate())
        const dates = yy + '-' + mm + '-' + dd
        console.log(dates)
      })
    })
  },
  mounted() {
    // this.notice = 'hello world'
    this.driver = new Driver()
    // Buss.$on('aaa', data => {
    //   console.log(data)
    //   this.notice = data
    //   console.log(this.notice)
    // })
    this.dataItem()
    this.solarDate2lunar()
  },
  methods: {
    dataItem() {
      const items = [
        {
          id: 1,
          name: '等于',
          table: 'staff',
          key_file: 'staff_id',
          file: 'rank_id',
          file_content: '职级',
          type: 'number',
          code: '=',
          process_type_id: 2,
          condition_list_table: null,
          infos_id: '1207504682260500480',
          app_id: '1151335644375683072',
          url: '',
          code_flag: 1,
          parameters: [],
          choose: 0,
          codes: [
            {
              keys: '<',
              val: '小于'
            },
            {
              keys: '>',
              val: '大于'
            },
            {
              key: '<=',
              val: '小于等于'
            },
            {
              keys: '=',
              val: '等于'
            },
            {
              keys: '>=',
              val: '大于等于'
            },
            {
              keys: 'in',
              val: '介于(两数之间)'
            }
          ],
          typeData: []
        },
        {
          id: 2,
          name: '等于',
          table: 'staff',
          key_file: 'staff_id',
          file: 'position_id',
          file_content: '职位',
          type: 'number',
          code: '=',
          process_type_id: 2,
          condition_list_table: null,
          infos_id: '1207504682260500480',
          app_id: '1151335644375683072',
          url: '',
          code_flag: 0,
          parameters: [],
          choose: 0,
          typeData: []
        },
        {
          id: 3,
          name: '等于',
          table: 'staff',
          key_file: 'staff_id',
          file: 'organization_id',
          file_content: '所属部门',
          type: 'checkbox',
          code: 'in',
          process_type_id: 2,
          condition_list_table: null,
          infos_id: '1207504682260500480',
          app_id: '1151335644375683072',
          url: 'http://newhrapi.17mine.net/test/index/test',
          code_flag: 0,
          parameters: [],
          choose: 0,
          typeData: [
            {
              keys: 1,
              val: '数据1'
            },
            {
              keys: 2,
              val: '数据2'
            },
            {
              keys: 3,
              val: '数据3'
            },
            {
              keys: 4,
              val: '数据4'
            }
          ]
        },
        {
          id: 4,
          name: '等于',
          table: 'staff',
          key_file: 'staff_id',
          file: 'rank_id',
          file_content: '职级1',
          type: 'input',
          code: '=',
          process_type_id: 2,
          condition_list_table: null,
          infos_id: '1207504682260500480',
          app_id: '1151335644375683072',
          url: '',
          code_flag: 0,
          parameters: [],
          choose: 0,
          typeData: []
        },
        {
          id: 5,
          name: '等于',
          table: 'staff',
          key_file: 'staff_id',
          file: 'position_id',
          file_content: '职位1',
          type: 'number',
          code: '=',
          process_type_id: 2,
          condition_list_table: null,
          infos_id: '1207504682260500480',
          app_id: '1151335644375683072',
          url: '',
          code_flag: 0,
          parameters: [],
          choose: 0,
          typeData: []
        },
        {
          id: 6,
          name: '等于',
          table: 'staff',
          key_file: 'staff_id',
          file: 'organization_id',
          file_content: '所属部门1',
          type: 'radio',
          code: '=',
          process_type_id: 2,
          condition_list_table: null,
          infos_id: '1207504682260500480',
          app_id: '1151335644375683072',
          url: 'http://newhrapi.17mine.net/test/index/test',
          code_flag: 0,
          parameters: [],
          choose: 0,
          typeData: [
            {
              keys: 1,
              val: '数据1'
            },
            {
              keys: 2,
              val: '数据2'
            },
            {
              keys: 3,
              val: '数据3'
            },
            {
              keys: 4,
              val: '数据4'
            }
          ]
        },
        {
          id: 7,
          name: '等于',
          table: 'staff',
          key_file: 'staff_id',
          file: 'rank_id',
          file_content: '职级2',
          type: 'select',
          code: '=',
          process_type_id: 2,
          condition_list_table: null,
          infos_id: '1207504682260500480',
          app_id: '1151335644375683072',
          url: 'http://newhrapi.17mine.net/test/index/test',
          code_flag: 0,
          parameters: [],
          choose: 0,
          typeData: [
            {
              keys: 1,
              val: '数据1'
            },
            {
              keys: 2,
              val: '数据2'
            },
            {
              keys: 3,
              val: '数据3'
            },
            {
              keys: 4,
              val: '数据4'
            }
          ]
        },
        {
          id: 33,
          name: '等于',
          table: 'staff',
          key_file: 'staff_id',
          file: 'organization_id',
          file_content: '所属部门',
          type: 'checkbox',
          code: 'in',
          process_type_id: 2,
          condition_list_table: null,
          infos_id: '1207504682260500480',
          app_id: '1151335644375683072',
          url: 'http://newhrapi.17mine.net/test/index/test',
          code_flag: 0,
          parameters: [],
          choose: 0,
          typeData: [
            {
              keys: 11,
              val: '数据11'
            },
            {
              keys: 21,
              val: '数据21'
            },
            {
              keys: 31,
              val: '数据31'
            },
            {
              keys: 4,
              val: '数据4'
            }
          ]
        }
      ]
      const arr = []
      items.map(el => {
        if (el.type === 'checkbox') {
          arr.push(el)
        }
      })
      // console.log(arr)
      this.items = arr
    },
    guide() {
      this.driver.defineSteps(steps)
      this.driver.start()
    },
    boxChange(data) {
      // console.log(data.typeData)
      const arr = []
      data.typeData.map(el => {
        arr.push(el.val)
      })
      this.bbb = [...new Set(arr)]
      console.log(this.bbb)
      this.aaa = data.parameters
      console.log(this.aaa)
      if (this.bbb.length === this.aaa.length) {
        console.log(data)
        // this.handleCheckAllChange(data)
        // this.isIndeterminate = false
      }
    },
    // 全选
    handleCheckAllChange(val) {
      console.log(val)
      debugger
      if (val.parameters[0] === undefined || val.parameters.length === 0) {
        val.typeData.map(el => {
          val.parameters.push(el.val)
        })
      } else {
        val.parameters = []
      }
      // this.isIndeterminate = false
    },
    // 日历
    solarDate2lunar(solarDate) {
      // console.log(solarDate)
      if (solarDate !== undefined) {
        var solar = solarDate.split('-')
        var lunar = calendar.solar2lunar(solar[0], solar[1], solar[2])
        // console.log(lunar)
        var ydate = lunar.IDayCn
        if (lunar.lunarFestival !== null) {
          ydate = lunar.lunarFestival
        }
        if (lunar.festival !== null) {
          ydate = lunar.festival
        }
        const obj = {
          date: solar[2] + '\n' + ydate,
          isLunarFestival: lunar.isLunarFestival,
          isFestval: lunar.isFestval
        }
        return obj
      }
    },
    checkTime(i) {
      if (i < 10) {
        i = '0' + i
      }
      return i
    },
    clickEveryone(e) {
      const yy = new Date(e).getFullYear()
      const mm = this.checkTime(new Date(e).getMonth() + 1)
      const dd = this.checkTime(new Date(e).getDate())
      const dates = yy + '-' + mm + '-' + dd
      console.log(dates)
    }
  }
}
</script>
<style lang="scss" scoped>
  .active {
    color: #f00;
  }
  .normal {
    white-space: pre-line;
    margin: 0;
    text-align: center;
  }
</style>

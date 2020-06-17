<template>
  <div>
    <div class="detail">
      <span ref="extend" class="extendAll active" @click="extendAll">全部展开</span>
      <span ref="fold" class="foldAll" @click="foldAll">全部收起</span>
    <!-- <span :loading="downloadLoading" @click='exportTable'>导出表格</span> -->
    </div>
    <el-table
      ref="multipleTable"
      :data="tableData"
      style="width: 100%"
      row-key="id"
      :tree-props="{children: 'children', hasChildren: 'hasChildren'}"
      @selection-change="handleSelectionChange"
      @row-click="clickRow"
    >
      <el-table-column
        type="selection"
        width="55"
      />
      <el-table-column
        prop="date"
        label="日期"
        width="180"
      >
        <!-- <template slot-scope="scope">
          <el-checkbox v-model="scope.row.checked" :label="scope.row.date" :key="scope.row.id" @change="handleCheckAllChange(scope.row)">{{scope.row.date}}</el-checkbox>
      </template> -->
      </el-table-column>
      <el-table-column
        prop="name"
        label="姓名"
        width="180"
      />
      <el-table-column
        prop="address"
        label="地址"
      />
    </el-table>

    <el-collapse v-model="activeNames" @change="handleChange">
      <el-collapse-item name="1222222">
        <template slot="title">
          <i class="titleleftbar" />
          <span>机构信息</span>
          <span>未完成</span>
        </template>
        <div>具体内容</div>
      </el-collapse-item>
      <el-collapse-item title="标题2" name="2">
        <div>控制反馈：通过界面样式和交互动效让用户可以清晰的感知自己的操作；</div>
        <div>页面反馈：操作后，通过页面元素的变化清晰地展现当前状态。</div>
      </el-collapse-item>
      <el-collapse-item title="标题3" name="3">
        <div>简化流程：设计简洁直观的操作流程；</div>
        <div>清晰明确：语言表达清晰且表意明确，让用户快速理解进而作出决策；</div>
        <div>帮助用户识别：界面简单直白，让用户快速识别而非回忆，减少用户记忆负担。</div>
      </el-collapse-item>
      <el-collapse-item title="标题4" name="4">
        <div>用户决策：根据场景可给予用户操作建议或安全提示，但不能代替用户进行决策；</div>
        <div>结果可控：用户可以自由的进行操作，包括撤销、回退和终止当前操作等。</div>
      </el-collapse-item>
    </el-collapse>
    <div>
      <el-tooltip class="item" effect="dark" :content="notices" placement="top-start">
        <el-button>上左</el-button>
      </el-tooltip>
      <el-progress :percentage="50" />
    </div>
    <div>**********************************************************************************************</div>
    <div>
      <span @click="downCode">导出二维码</span>
    </div>
    <div>**************************************表格内编辑***********************************************</div>
    <div class="tableContent">
      <div class="tabLeft">
        <svg-icon class="loop" icon-class="money" />
        <span>有害垃圾</span>
      </div>
      <el-table
        ref="tables"
        class="tables"
        :data="tableDataEdit"
      >
        <el-table-column
          prop="id"
          label="ID"
          width="180"
        />
        <el-table-column
          prop="name"
        >
          <template
            slot="header"
          >
            <i class="star" />
            <span>
              姓名
            </span>
          </template>
          <template slot-scope="scope">
            <span>分类收集</span>
            <el-input v-if="scope.row.disabled" v-model="scope.row.name" size="mini" style="width:100px;" :disabled="true" />
            <el-input v-else v-model="scope.row.name" size="mini" style="width:100px;" />
          </template>
        </el-table-column>
        <el-table-column
          prop="amount1"
        >
          <template
            slot="header"
          >
            <i class="star" />
            <span>
              数值 1（元）
            </span>
          </template>
        </el-table-column>
        <el-table-column
          prop="amount2"
          label="数值 2（元）"
        />
        <el-table-column
          prop="amount3"
          label="数值 3（元）"
        />
      </el-table>
    </div>
    <div>*****************************兄弟组件传值bus**************************</div>
    <!-- <el-button type='primary' size='mini' @click='toBrother'>向兄弟组件传值</el-button> -->
    <public-module :content-obj="contentObj" />
    <!-- 百度地图 -->
    <div id="map" style="width:800px;height:400px;" />
    <!-- 时间控件 -->
    <el-time-picker
      is-range
      v-model="value1"
      value-format='HH:mm'
      format='HH:mm'
      range-separator="至"
      start-placeholder="开始时间"
      end-placeholder="结束时间"
      placeholder="选择时间范围">
    </el-time-picker>
  </div>
</template>
<script>
import PublicModule from '@/components/PublicModule'
// import qscode from 'qrcode'
import Buss from '@/utils/bus'
export default {
  components: {
    // qscode
    PublicModule
  },
  data() {
    return {
      contentObj: {
        title: '微信真人阅读',
        price: 0.095,
        count: 100,
        notice: 'xxxxxxxxx'
      },
      value: '100',
      shareUrl: {},
      downloadLoading: false,
      notices: '',
      activeNames: ['1222222'],
      multipleSelection: [],
      tableData: [{
        id: 1,
        type: 1,
        date: '2016-05-02',
        name: '王小虎',
        address: '上海市普陀区金沙江路 1518 弄'
      }, {
        id: 2,
        date: '2016-05-04',
        name: '王小虎',
        address: '上海市普陀区金沙江路 1517 弄'

      }, {
        id: 3,
        date: 'AAA',
        name: '王小虎',
        address: '上海市普陀区金沙江路 1519 弄',
        children: [
          {
            id: 31,
            date: '2016-05-01',
            name: '王小虎',
            address: '上海市普陀区金沙江路 31 弄'
          }, {
            id: 32,
            date: '2016-05-01',
            name: '王小虎',
            address: '上海市普陀区金沙江路 32 弄'
          }
        ]
      }, {
        id: 4,
        date: 'BBB',
        name: '王小虎',
        address: '上海市普陀区金沙江路 1516 弄',
        children: [
          {
            id: 41,
            date: '2016-05-01',
            name: '王小虎',
            address: '上海市普陀区金沙江路 31 弄'
          }, {
            id: 42,
            date: '2016-05-01',
            name: '王小虎',
            address: '上海市普陀区金沙江路 32 弄'
          }
        ]
      },
      {
        id: 5,
        date: 'CCC',
        name: '王小虎',
        address: '上海市普陀区金沙江路 1516 弄',
        children: [
          {
            id: 51,
            date: '2016-05-01',
            name: '王小虎',
            address: '上海市普陀区金沙江路 31 弄'
          }, {
            id: 52,
            date: '2016-05-01',
            name: '王小虎',
            address: '上海市普陀区金沙江路 32 弄'
          }
        ]
      }
      ],
      tableDataEdit: [
        {
          id: '12987122',
          name: '王小虎',
          amount1: '234',
          amount2: '3.2',
          amount3: 10,
          disabled: true
        }, {
          id: '12987123',
          name: '王小虎',
          amount1: '165',
          amount2: '4.43',
          amount3: 12,
          disabled: true
        }, {
          id: '12987124',
          name: '王小虎',
          amount1: '324',
          amount2: '1.9',
          amount3: 9,
          disabled: false
        }
      ],
      value1: ['00:00:00', '23:59:59']
    }
  },
  mounted() {
    // this.extendAll()
    this.getNotices()
    Buss.$emit('aaa', '1111')
    this.creatBPM()
  },
  methods: {
    getNotices() {
      // console.log('bus', Buss)
      this.notices = '这是提示文字'
    },
    handleSelectionChange(val) {
      console.log(val)
      this.multipleSelection = val
      // console.log(this.multipleSelection)
    },
    handleChange(val) {
      console.log('折叠面板', val)
    },
    extendAll() {
      // debugger
      this.$refs.extend.className = 'extendAll active'
      this.$refs.fold.className = 'foldAll'
      const extendEls = document.getElementsByClassName('el-table__expand-icon--expanded')
      // console.log('extendEls', extendEls)
      const els = document.getElementsByClassName('el-table__expand-icon')
      // console.log('els', els)
      for (let i = 0; i < els.length; i++) {
        if (extendEls.length === 0) {
          els[i].click()
        } else if (extendEls.length > 0 && extendEls.length < els.length) {
          // debugger
          console.log(els[i].attributes.class.value)
          if (els[i].attributes.class.value === 'el-table__expand-icon el-table__expand-icon--expanded') {
            console.log('11')
          } else {
            console.log('22')
            els[i].click()
          }
        }
      }
    },
    foldAll() {
      this.$refs.fold.className = 'foldAll active'
      this.$refs.extend.className = 'extendAll'
      const extendEls = document.getElementsByClassName('el-table__expand-icon--expanded')
      // console.log('extendEls', extendEls)
      if (extendEls.length !== 0) {
        for (let i = 0; i < extendEls.length; i++) {
          extendEls[i].click()
        }
      }
    },
    downCode() {
      const a = document.createElement('a')
      a.href = 'http://192.168.2.252:7080/disposal/scales/getQRCode?scalesId=11111111111'
      a.download = '1111'
      a.click()
      this.$message({ message: '亲，正在进行下载保存', type: 'warning' })
    },
    clickRow(row, column, event) {
      console.log(row)
    },
    creatBPM() {
      // 百度地图API功能
      // var map = new BMap.Map('map') // 创建Map实例
      // var mPoint = new BMap.Point(116.404, 39.915)
      // map.enableScrollWheelZoom()
      // map.centerAndZoom(mPoint, 15)

      // var circle = new BMap.Circle(mPoint, 1000, { fillColor: 'blue', strokeWeight: 1, fillOpacity: 0.3, strokeOpacity: 0.3 })
      // map.addOverlay(circle)
      // var local = new BMap.LocalSearch(map, { renderOptions: { map: map, autoViewport: false }})
      // local.searchNearby('奥城c4', mPoint, 1000)
    }
  }
}
</script>
<style lang = 'scss'>
  .tables{
    /* &:before {
      height：0
    } */
    td,th {
      border:0 !important;
    }
  }
</style>
<style lang="scss" scoped>
  .detail{
    span{
      display: inline-block;
      height:30px;
      line-height: 30px;
      padding:0 20px;
      font-size:14px;
      border:1px solid #ccc;
    }
    .extendAll{
      border-right:0;
      position: relative;
      right: -4px;
      border-top-left-radius: 15px;
      border-bottom-left-radius: 15px;
    }
    .foldAll{
      border-top-right-radius: 15px;
      border-bottom-right-radius: 15px;
    }
    .active{
      background: #accbf5;
      color:#fff
    }
  }
  .tableContent{
    display: flex;
    justify-content: space-between;
  }
  .tabLeft{
    padding:10% 15px 0 15px;
  }
  .star{
    display: inline-block;
    width:12px;
    height: 12px;
    background: url('../../icons/required.png') no-repeat;
    background-size: 12px;
  }
  #map {
    margin-top: 20px;
    height: 700px;
  }
</style>

<template>
  <div class="app-container">
    <switch-roles @change="handleRolesChange" />
    <h1>动态添加表单元素</h1>
    <button type="primary" @click="add">添加表单元素</button>
    <el-form ref="formRule" :model="form" label-width="80px">
      <el-form-item :label="item.title" v-for="(item, index) in formList" :key='index'>
        <div v-html='item.content'></div>
      </el-form-item>
    </el-form>
    <el-drawer
      ref="drawer"
      title="添加模板字段"
      :visible.sync="dialog"
      direction="rtl"
      custom-class="demo-drawer"
    >
      <div class="demo-drawer__content">
        <el-form :model="form">
          <el-form-item label="活动区域" :label-width="formLabelWidth">
            <el-select v-model="form.region" placeholder="请选择">
              <el-option v-for="item in options" :key="item.path" :value="item.value" :label="item.title" />
            </el-select>
          </el-form-item>
        </el-form>
        <div class="demo-drawer__footer">
          <el-button @click="cancelForm">取 消</el-button>
          <el-button type="primary" @click="saveForm">确 定</el-button>
        </div>
      </div>
    </el-drawer>
  </div>
</template>

<script>
import SwitchRoles from './components/SwitchRoles'

export default {
  name: 'PagePermission',
  components: { SwitchRoles },
  data() {
    return {
      form: {
        region: ''
      },
      formLabelWidth: 'right',
      dialog: false,
      options: [
        {
          title: '姓名',
          path: '1',
          value: 'name',
          type: 'input',
          place: '请输入'
        },
        {
          title: '性别',
          path: '2',
          value: 'sex',
          type: 'radio',
          place: ''
        }
      ],
      formList: [],
    }
  },
  methods: {
    handleRolesChange() {
      this.$router.push({ path: '/permission/index?' + +new Date() })
    },
    add() {
      this.dialog = true
    },
    cancelForm() {

    },
    saveForm() {
      // console.log(this.form.region)
      const aa = this.options.filter(el => el.value == this.form.region)
      // console.log(aa)
      if (aa[0].type == 'input') {
        aa[0].content = "<el-input v-model='formRule.name' :placeholder='item.place' style='width:200px; height:40px'></el-input>"
      }
      this.formList.push(aa[0])
      console.log(this.formList)
      this.dialog = false
    }
  }
}
</script>

<template>
  <div>
    <button type="primary" @click="add">添加表单元素</button>
    <el-drawer
      ref="drawer"
      title="添加模板字段"
      :visible.sync="dialog"
      direction="rtl"
      custom-class="demo-drawer"
    >
      <div class="demo-drawer__content">
        <el-form :model="form">
          <el-form-item label="字段类型" :label-width="formLabelWidth">
            <el-select v-model="form.region" placeholder="请选择">
              <el-option
                v-for="item in options"
                :key="item.key"
                :value="item.value"
                :label="item.title"
              />
            </el-select>
          </el-form-item>
        </el-form>
        <Input v-if="form.region == 'name'" />
        <div class="demo-drawer__footer">
          <el-button @click="cancelForm">取 消</el-button>
          <el-button type="primary" @click="saveForm">确 定</el-button>
        </div>
      </div>
    </el-drawer>
  </div>
</template>
<script>
import Input from './components/edit-item/input'
export default {
  components: {
    Input
  },
  data() {
    return {
      dialog: false,
      form: {
        region: ''
      },
      formLabelWidth: 'right',
      options: [
        {
          title: '姓名',
          key: '1',
          value: 'name',
          type: 'input'
        },
        {
          title: '性别',
          key: '2',
          value: 'sex',
          type: 'radio'
        }
      ],
      formList: []
    }
  },
  methods: {
    add() {
      this.dialog = true
    },
    cancelForm() {
      this.dialog = false
    },
    saveForm() {
      this.dialog = false
    }
  }
}
</script>
<style lang="scss" scoped>
  .demo-drawer__footer {
    position: fixed;
    bottom: 10px;
    right: 10%;
  }
</style>

<template>
  <div>
    <el-form :model="formIpt" :label-width="formLabelWidth">
      <el-form-item label="标题">
        <span>姓名</span>
      </el-form-item>
      <el-form-item label="提示文字">
        <el-input
          v-model="formIpt.place"
          placeholder="请输入提示文字"
          maxlength="10"
          style="width:200px;"
          size="small"
        ></el-input>
        <span>不超过10个字</span>
      </el-form-item>
    </el-form>
  </div>
</template>
<script>
export default {
  data() {
    return {
      formIpt: {
        place: ''
      },
      formLabelWidth: 'right'
    }
  }
}
</script>
<style lang="scss" scoped>
</style>

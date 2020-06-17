<template>
  <div>
    <div class="titleV"><span>{{ contentObj.title }}</span><span>(单价：{{ contentObj.price }}元/个)</span></div>
    <el-form ref="ruleForm" :model="ruleForm" :rules="rules" class="ruleForm">
      <el-form-item>
        <template slot-scope="scope">
          <div>下单地址</div>
          <el-input v-model="ruleForm.name" placeholder="请正确填写链接地址" />
        </template>
      </el-form-item>
      <el-form-item>
        <template slot-scope="scope">
          <div>下单数量</div>
          <el-input v-model="ruleForm.count" :placeholder="pvalue" />
        </template>
      </el-form-item>
      <el-form-item prop="name">
        <template slot-scope="scope">
          <div>说明</div>
          <div>{{ contentObj.notice }}</div>
        </template>
      </el-form-item>
    </el-form>
  </div>
</template>
<script>
export default {
  name: 'PublicMoudle',
  props: {
    contentObj: {
      type: Object,
      required: true
    }
  },
  data() {
    return {
      ruleForm: {
        name: '',
        count: ''
      },
      pvalue: '',
      rules: {}
    }
  },
  mounted() {
    console.log(this.contentObj)
    this.pvalue = '最低下单数：' + this.contentObj.count + '个起'
  },
  methods: {

  }
}
</script>
<style lang="scss" scoped>

</style>

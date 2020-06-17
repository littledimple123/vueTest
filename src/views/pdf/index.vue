<template>
  <div class="app-container">
    <aside style="margin-top:15px;">
      Here we use window.print() to implement the feature of downloading PDF.
    </aside>
    <router-link target="_blank" to="/pdf/download">
      <el-button type="primary">
        Click to download PDF
      </el-button>
    </router-link>
    <el-button @click='aa'>查看富文本编辑器数据</el-button>
    <!-- 富文本编辑器 -->
    <div class="edit_container">
      <quill-editor
        v-model="content"
        ref="myQuillEditor"
        :options="editorOption"
        style="width:800px;height:200px"
        @change="editorChange($event)"
      >
      </quill-editor>
    </div>

  </div>
</template>
<script>
import { quillEditor } from 'vue-quill-editor'
import 'quill/dist/quill.snow.css'
import * as Quill from 'quill'
export default {
  name: 'Pdf',
  components: {
    quillEditor
  },
  data() {
    return {
      content: '',
      editorOption: {
        theme: 'snow',
        placeholder: '请输入正文'
      }
    }
  },
  mounted() {
  },
  methods: {
    editorChange(e) {
      this.content = e.html
    },
    aa() {
      console.log(this.content)
    }
  }
}
</script>
<style lang="scss" scoped>

</style>

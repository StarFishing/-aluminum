<template>
  <div v-if="dialogFormVisible"
       class="limitsWrapper">
    <el-dialog title="修改权限"
               :visible.sync="dialogFormVisible"
               width="500px">
      <div slot="title"
           class="header-title"
           style="pading: 0 10px">
        <i class="el-icon-edit"
           style="color:#409EFF" />
        <span style="margin-left:10px">修改权限</span>
      </div>
      <el-form ref="ruleForm"
               :model="ruleForm"
               :rules="rules">
        <el-form-item label="身份"
                      :label-width="formLabelWidth"
                      prop="identity">
          <el-select v-model="ruleForm.identity"
                     placeholder="设置身份"
                     style="width:250px">
            <el-option label="管理员"
                       value="管理员" />
            <el-option label="普通用户"
                       value="普通用户" />
            <el-option label="密保员"
                       value="密保员" />
          </el-select>
        </el-form-item>
        <el-form-item label="等级"
                      :label-width="formLabelWidth"
                      prop="level">
          <el-select v-model="ruleForm.level"
                     placeholder="选择等级"
                     style="width:250px">
            <el-option label="1"
                       value="1" />
            <el-option label="2"
                       value="2" />
            <el-option label="3"
                       value="3" />
          </el-select>
        </el-form-item>
      </el-form>
      <div slot="footer"
           class="dialog-footer">
        <el-button @click="cancel">取 消</el-button>
        <el-button type="primary"
                   :loading="loading"
                   @click="submit">确 定</el-button>
      </div>
    </el-dialog>
  </div>
</template>
<script>
export default {
  data() {
    return {
      ruleForm: {
        identity: '',
        level: ''
      },
      rules: {
        identity: [
          { required: true, message: '选择身份', trigger: 'change' }
        ],
        level: [
          { required: true, message: '设置等级', trigger: 'change' }
        ]
      },
      formLabelWidth: '100px',
      dialogFormVisible: false,
      loading: false
    }
  },
  methods: {
    cancel() {
      this.$refs['ruleForm'].resetFields()
      this.dialogFormVisible = false
      this.$message({
        message: '取消成功',
        type: 'success'
      })
    },
    submit() {
      console.log(this.form)
      this.loading = true
      this.$refs['ruleForm'].validate((valid) => {
        if (valid) {
          setTimeout(() => {
            this.loading = false
            this.$refs['ruleForm'].resetFields()
            this.dialogFormVisible = false
          }, 1000)
        } else {
          this.loading = false
          this.$message({
            message: '验证失败',
            type: 'error'
          })
          return false
        }
      })
    },
    isshow(flag) {
      this.dialogFormVisible = flag
    }
  }
}
</script>
<style>
.limitsWrapper {
  overflow: hidden;
}
.limitsWrapper .el-dialog {
  border-radius: 10px;
}
</style>

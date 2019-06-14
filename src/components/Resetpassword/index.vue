<template>
  <div class="passwordWrapper">

    <el-dialog
      title="修改密码"
      :visible.sync="passwordDialog"
      width="400px"
    >
      <div slot="title" class="header-title" style="pading: 0 10px">
        <i class="el-icon-info" style="color:#E6A23C" />
        <span style="margin-left:20px">修改密码</span>
      </div>
      <el-form ref="ruleForm" :model="ruleForm" status-icon :rules="rules" label-width="100px" class="demo-ruleForm">
        <el-form-item label="密码" prop="pass">
          <el-input v-model="ruleForm.pass" type="password" autocomplete="off" />
        </el-form-item>
        <el-form-item label="确认密码" prop="checkPass">
          <el-input v-model="ruleForm.checkPass" type="password" autocomplete="off" />
        </el-form-item>
        <el-form-item>
          <el-button type="primary" :loading="loading" @click="submitForm('ruleForm')">确认</el-button>
          <el-button @click="resetForm('ruleForm')">取消</el-button>
        </el-form-item>
      </el-form>
    </el-dialog>

  </div>
</template>

<script>
export default {
  // props: {
  //   passwordobj: {
  //     type: Object,
  //     default: null
  //   }
  // },
  data() {
    // this.passwordDialog = this.passwordobj.show
    var validatePass = (rule, value, callback) => {
      if (value === '') {
        callback(new Error('请输入密码'))
      } else {
        if (this.ruleForm.checkPass !== '') {
          this.$refs.ruleForm.validateField('checkPass')
        }
        callback()
      }
    }
    var validatePass2 = (rule, value, callback) => {
      if (value === '') {
        callback(new Error('请再次输入密码'))
      } else if (value !== this.ruleForm.pass) {
        callback(new Error('两次输入密码不一致!'))
      } else {
        callback()
      }
    }
    return {
      passwordDialog: false,
      loading: false,
      ruleForm: {
        pass: '',
        checkPass: ''
      },
      rules: {
        pass: [
          { validator: validatePass, trigger: 'blur' }
        ],
        checkPass: [
          { validator: validatePass2, trigger: 'blur' }
        ]
      }
    }
  },
  // watch: {
  //   passwordobj(oldValue, newValue) {
  //     console.log(oldValue)
  //     console.log(newValue)
  //     this.passwordDialog = this.passwordobj.show
  //   }
  // },
  methods: {
    getShow(obj) {
      this.passwordDialog = obj.show
    },
    submitForm(formName) {
      this.$refs[formName].validate((valid) => {
        if (valid) {
          this.loading = true
          setTimeout(() => {
            this.loading = false
            this.passwordDialog = false
          }, 1000)
        } else {
          this.loading = false
          console.log('error submit!!')
          return false
        }
      })
    },
    resetForm(formName) {
      this.$refs[formName].resetFields()
      this.passwordDialog = false
    }
  }
}
</script>

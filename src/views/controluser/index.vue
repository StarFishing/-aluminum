<template>
  <div>
    <div style="margin: 20px 40px 0 40px">
      <el-button type="primary"
                 icon="el-icon-plus"
                 @click="adduser">新增用户</el-button>
    </div>
    <div class="userWrapper">

      <el-table :data="tableData.filter(data => !search || data.name.toLowerCase().includes(search.toLowerCase()))"
                style="width: 100%"
                max-height="600">
        <el-table-column label="角色"
                         prop="role" />
        <el-table-column label="姓名"
                         prop="name" />
        <el-table-column label="密级"
                         prop="secret" />
        <el-table-column label="部门"
                         prop="department" />
        <el-table-column label="性别"
                         prop="sex" />
        <el-table-column align="right"
                         width="300">
          <!--eslint-disable -->
          <template slot="header"
                    slot-scope="scope">
            <el-input v-model="search"
                      size="mini"
                      placeholder="输入关键字搜索" />
          </template>
          <template slot-scope="scope">
            <el-button size="mini"
                       @click="handleEdit(scope.$index, scope.row)">编辑</el-button>
            <el-button size="mini"
                       type="primary"
                       @click="handlepassword(scope.$index, scope.row)">密码</el-button>
            <el-button size="mini"
                       type="danger"
                       @click="handleDelete(scope.$index, scope.row)">删除</el-button>
          </template>
        </el-table-column>
      </el-table>
      <el-dialog title="用户"
                 :visible.sync="adduserDialog"
                 width="500px"
                 id="identy">
        <div slot="title"
             class="header-title"
             style="pading: 0 10px">
          <i v-if="editorflag"
             class="el-icon-info"
             style="color:#E6A23C" />
          <i v-if="!editorflag"
             class="el-icon-circle-plus"
             style="color:#35affb" />
          <span style="margin-left:20px">{{editorflag?'编辑用户':'添加用户'}}</span>
        </div>
        <el-form ref="ruleForm"
                 :model="ruleForm"
                 :rules="rules"
                 label-width="100px"
                 class="demo-ruleForm">
          <el-form-item label="用户名"
                        prop="name"
                        v-if="!editorflag">
            <el-input v-model="ruleForm.name"
                      style="width: 200px;" />
          </el-form-item>
          <el-form-item label="权限"
                        prop="limite">
            <el-select v-model="ruleForm.limite"
                       placeholder="请选择活动区域">
              <el-option label="密保员"
                         value="密保员" />
              <el-option label="普通用户"
                         value="普通用户" />
            </el-select>
          </el-form-item>
          <el-form-item label="密级"
                        prop="secrelever">
            <el-select v-model="ruleForm.secrelever"
                       placeholder="选择密级">
              <el-option label="一级"
                         value="1" />
              <el-option label="二级"
                         value="2" />
              <el-option label="三级"
                         value="3" />
            </el-select>
          </el-form-item>
          <el-form-item label="登录密码"
                        prop="password">
            <el-input v-model="ruleForm.password"
                      style="width: 200px;"
                      type="password" />
          </el-form-item>
          <el-form-item label="确认密码"
                        prop="repassword">
            <el-input v-model="ruleForm.repassword"
                      style="width: 200px;"
                      type="password" />
          </el-form-item>
          <el-form-item label="职责描述"
                        prop="desc">
            <el-input v-model="ruleForm.desc"
                      type="textarea"
                      style="width: 200px;" />
          </el-form-item>
          <el-form-item>
            <el-button type="primary"
                       @click="submitForm('ruleForm')">创建用户</el-button>
            <el-button @click="resetForm('ruleForm')">取消</el-button>
          </el-form-item>
        </el-form>

      </el-dialog>
      <el-dialog title="警告"
                 :visible.sync="centerDialogVisible"
                 width="30%"
                 center>
        <span>你将要删除用户{{ username }}</span>
        <span slot="footer"
              class="dialog-footer">
          <el-button @click="centerDialogVisible = false">取 消</el-button>
          <el-button type="primary"
                     :loading="loading"
                     @click="sureCommit">确 定</el-button>
        </span>
      </el-dialog>
      <reset-password ref="pass" />
    </div>
  </div>

</template>
<script>
import resetPassword from '@/components/Resetpassword/index'
export default {
  components: {
    resetPassword
  },
  data() {
    return {
      tableData: [{
        role: '管理员',
        name: '王小虎',
        address: '上海市普陀区金沙江路 1518 弄',
        secret: '4',
        department: '人事',
        sex: '男'
      }, {
        role: '普通用户',
        name: '王小虎',
        address: '上海市普陀区金沙江路 1517 弄',
        secret: '4',
        department: '人事',
        sex: '男'
      }, {
        role: '部长',
        name: '张小虎',
        address: '上海市普陀区金沙江路 1519 弄',
        secret: '4',
        department: '人事',
        sex: '男'
      }, {
        role: '平民',
        name: '王小虎',
        address: '上海市普陀区金沙江路 1516 弄',
        secret: '4',
        department: '人事',
        sex: '男'
      }],
      search: '',
      centerDialogVisible: false,
      adduserDialog: false,
      editorflag: false,
      loadingeditor: false,
      loading: false,
      passwordobj: {},
      username: '',
      ruleForm: {
        name: '',
        limite: '普通用户',
        secrelever: '',
        password: '',
        repassword: '',
        delivery: false,
        type: [],
        resource: '',
        desc: ''
      },
      rules: {
        name: [
          { required: true, message: '用户户名', trigger: 'blur' },
          { min: 2, max: 14, message: '长度在 6 到 14 个字符', trigger: 'blur' }
        ],
        limite: [
          { required: true, message: '设置权限', trigger: 'change' }
        ],
        secrelever: [
          { required: true, message: '设置密级', trigger: 'change' }
        ],
        password: [
          { required: true, message: '输入密码', trigger: 'blur' }
        ],
        repassword: [
          { required: true, message: '再次输入密码', trigger: 'blur' }
        ],
        desc: [
          { required: true, message: '请填职责', trigger: 'blur' }
        ]
      }
    }
  },
  created() {
    console.log('data request')
  },
  methods: {
    sureCommit() {
      this.loading = true
      this.close()
    },
    editorsure() {
      this.loadingeditor = true
    },
    adduser() {
      this.editorflag = false
      this.adduserDialog = true
    },
    handleEdit(index, row) {
      this.adduserDialog = true
      this.editorflag = true
      console.log(index, row)
    },
    handlepassword(index, row) {
      this.passwordobj.show = true
      this.passwordobj.name = row.name
      this.$refs.pass.getShow(this.passwordobj)
      console.log(this.passwordobj)
    },
    handleDelete(index, row) {
      this.centerDialogVisible = true
      this.username = row.name
      console.log(index, row)
    },
    close() {
      this.$confirm('确认删除？')
        .then(_ => {
          this.centerDialogVisible = false
          this.loading = false
          this.$message({
            message: '恭喜你删除成功',
            type: 'success'
          })
        })
        .catch(_ => {
          this.loading = false
        })
    },
    submitForm(formName) {
      this.$refs[formName].validate((valid) => {
        if (valid) {
          this.adduserDialog = false
          alert('submit!')
        } else {
          console.log('error submit!!')
          return false
        }
      })
    },
    resetForm(formName) {
      this.adduserDialog = false
      this.$refs[formName].resetFields()
    }

  }
}
</script>
<style>
.userWrapper {
  margin: 20px 40px 0 40px;
  border: 1px solid #5a5f6561;
  overflow: hidden;
  border-radius: 8px;
}
.userWrapper .el-dialog {
  transform: none;
  left: 0;
  position: relative;
  margin: 0 auto;
  border-radius: 10px;
}
.userWrapper #identy .el-dialog__body {
  padding: 30px 20px;
  color: #606266;
  font-size: 14px;
  padding-left: 70px;
}
</style>

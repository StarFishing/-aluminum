<template>
  <div class="roleWrapper">
    <el-table :data="rolesList"
              style="width: 100%;margin-top:30px;"
              border>
      <el-table-column align="center"
                       label="身份"
                       width="220">
        <template slot-scope="scope">
          {{ scope.row.key }}
        </template>
      </el-table-column>
      <el-table-column align="center"
                       label="用户名"
                       width="220">
        <template slot-scope="scope">
          {{ scope.row.name }}
        </template>
      </el-table-column>
      <el-table-column align="header-center"
                       label="职责">
        <template slot-scope="scope">
          {{ scope.row.description }}
        </template>
      </el-table-column>
      <el-table-column align="center"
                       label="Operations">
        <template slot-scope="scope">
          <el-button type="primary"
                     size="small"
                     @click="handleEdit(scope)">
            编辑权限
          </el-button>
          <el-button type="danger"
                     size="small"
                     @click="handleDelete(scope)">
            删除
          </el-button>
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>
<script>
export default {
  data() {
    return {
      rolesList: [
        {
          key: 'admin',
          name: 'admin',
          description: 'hello'
        }
      ]
    }
  },
  methods: {
    handleEdit(scope) {
      console.log(scope.row.name)
    },
    handleDelete(cope) {
      console.log('delet')
    }
  }
}
</script>
<style scope>
.roleWrapper {
  padding: 20px;
}
</style>

<template>
  <el-container>
    <!-- <el-header>Header</el-header> -->
    <el-header style="font-size: 40px; background-color: rgb(238, 241, 246)">Quiz后台管理</el-header>
    <el-container>
      <el-aside width="200px" style="background-color: rgb(238, 241, 246)">
        <el-menu :default-openeds="['1']">
          <el-submenu index="1">
            <template slot="title">管理选项</template>
            <el-menu-item-group>
              <el-menu-item index="1-1"><router-link to="/user">用户管理</router-link></el-menu-item>
              <el-menu-item index="1-2"><router-link to="/question">题目管理</router-link></el-menu-item>
            </el-menu-item-group>

            <!-- <el-menu-item-group title="分组2">
              <el-menu-item index="1-3">选项3</el-menu-item>
            </el-menu-item-group> -->
            <!-- <el-submenu index="1-4">
              <template slot="title">选项4</template>
              <el-menu-item index="1-4-1">选项4-1</el-menu-item>
            </el-submenu> -->
          </el-submenu>
        </el-menu>
      </el-aside>

      <el-main>
        <!-- 顶部表单：查询 / 添加 -->
        <el-form :inline="true" :model="formInline" class="demo-form-inline">
          <el-form-item label="用户名">
            <el-input v-model="formInline.name" placeholder="请输入用户名"></el-input>
          </el-form-item>
          <el-form-item>
            <el-button type="primary" @click="handleSearch">查询</el-button>
            <el-button type="success" @click="handleAdd">添加用户</el-button>
          </el-form-item>
        </el-form>

        <!-- 显示的table -->
        <el-table :data="tableData" style="width: 100%">
          <el-table-column label="日期" width="300">
            <template slot-scope="scope">
              <i class="el-icon-time"></i>
              <span style="margin-left: 10px">{{ scope.row.date }}</span>
            </template>
          </el-table-column>

          <el-table-column label="姓名" width="300">
            <template slot-scope="scope">
              <div slot="reference" class="name-wrapper">
                <el-tag size="medium">{{ scope.row.name }}</el-tag>
              </div>
            </template>
          </el-table-column>

          <el-table-column label="密码" width="200">
            <template slot-scope="scope">
              <span>{{ scope.row.password }}</span>
            </template>
          </el-table-column>

          <el-table-column label="操作">
            <template slot-scope="scope">
              <el-button size="mini" @click="handleEdit(scope.row)">编辑</el-button>
              <el-button size="mini" type="danger" @click="handleDelete(scope.row)">删除</el-button>
            </template>
          </el-table-column>
        </el-table>

        <!-- 分页 -->
        <el-pagination
          class="pagination"
          background
          layout="prev, pager, next, jumper"
          :total="total"
          :page-size="pageSize"
          :current-page.sync="currentPage"
        >
        </el-pagination>

        <!-- 添加/编辑用户对话框 -->
        <el-dialog title="添加新用户" :visible.sync="dialogVisible" width="500px">
          <el-form :model="form" label-width="80px">
            <el-form-item label="用户名">
              <el-input v-model="form.name"></el-input>
            </el-form-item>
            <el-form-item label="密码">
              <el-input v-model="form.password" type="password"></el-input>
            </el-form-item>
            <el-form-item label="确认密码">
              <el-input v-model="form.confirmPassword" type="password"></el-input>
            </el-form-item>
          </el-form>
          <div slot="footer">
            <el-button @click="dialogVisible = false">取 消</el-button>
            <el-button type="primary" @click="handleSubmit">确 定</el-button>
          </div>
        </el-dialog>
      </el-main>
    </el-container>
  </el-container>
</template>

<script>
export default {
  name: "UserView",
  data() {
    return {
      formInline: {
        name: ""
      },
      currentPage: 1,
      pageSize: 4,
      total: 400,
      dialogVisible: false,
      form: {
        id: null,
        date: "",
        name: "",
        password: "",
        confirmPassword: ""
      },
      tableData: [
        { id: 1, date: "2016-05-02", name: "王小虎", password: "123456" },
        { id: 2, date: "2016-05-04", name: "王小虎", password: "123456" },
        { id: 3, date: "2016-05-01", name: "王小虎", password: "123456" },
        { id: 4, date: "2016-05-03", name: "王小虎", password: "123456" }
      ]
    };
  },
  methods: {
    handleSearch() {
      this.currentPage = 1;
    },
    handleAdd() {
      this.form = { id: null, date: "", name: "", password: "", confirmPassword: "" };
      this.dialogVisible = true;
    },
    handleEdit() {
      // 保留弹窗功能，但不回显该行内容
      this.form = { id: null, date: "", name: "", password: "", confirmPassword: "" };
      this.dialogVisible = true;
    },
    handleDelete() {},
    handleSubmit() {
      this.dialogVisible = false;
    }
  }
};
</script>

<style scoped>
.demo-form-inline {
  margin-bottom: 10px;
}

.pagination {
  margin-top: 15px;
}

.name-wrapper {
  display: inline-block;
}
</style>

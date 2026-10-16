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
          <el-form-item label="题目">
            <el-input v-model="formInline.question" placeholder="请输入题目关键词"></el-input>
          </el-form-item>
          <el-form-item>
            <el-button type="primary" @click="handleSearch">查询</el-button>
            <el-button type="success" @click="dialogFormVisible = true">添加题目</el-button>
          </el-form-item>
        </el-form>

        <!-- 显示的table -->
        <el-table :data="tableData" style="width: 80%">
          <el-table-column label="序号" width="50">
            <template slot-scope="scope">
              <!-- <i class="el-icon-time"></i> -->
              <span style="margin-left: 10px">{{ scope.row.id }}</span>
            </template>
          </el-table-column>

          <el-table-column label="题目" width="250">
            <template slot-scope="scope">
              <!-- <el-popover trigger="hover" placement="top"> -->
                <!-- <p>姓名: {{ scope.row.name }}</p>
                <p>住址: {{ scope.row.address }}</p> -->
                <div slot="reference" class="name-wrapper">
                  <span style="margin-left: 10px">{{ scope.row.question }}</span>
                  <!-- <el-tag size="medium">{{ scope.row.question }}</el-tag> -->
                </div>
              <!-- </el-popover> -->
            </template>
          </el-table-column>

          <el-table-column label="选项" width="220">
            <template slot-scope="scope">
              <span>{{ scope.row.options }}</span>
            </template>
          </el-table-column>

          <el-table-column label="答案" width="120">
            <template slot-scope="scope">
              <span>{{ scope.row.answer }}</span>
            </template>
          </el-table-column>

          <el-table-column label="操作" width="200">
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

        <!-- 添加新题目对话框 -->
        <el-dialog title="添加新题目" :visible.sync="dialogFormVisible">
          <el-form :model="form">
            <el-form-item label="题目" :label-width="formLabelWidth">
              <el-input v-model="form.question" autocomplete="off"></el-input>
            </el-form-item>

            <el-form-item label="选项a" :label-width="formLabelWidth">
              <el-input v-model="form.optiona" autocomplete="off"></el-input>
            </el-form-item>

            <el-form-item label="选项b" :label-width="formLabelWidth">
              <el-input v-model="form.optionb" autocomplete="off"></el-input>
            </el-form-item>

            <el-form-item label="选项c" :label-width="formLabelWidth">
              <el-input v-model="form.optionc" autocomplete="off"></el-input>
            </el-form-item>

            <el-form-item label="选项d" :label-width="formLabelWidth">
              <el-input v-model="form.optiond" autocomplete="off"></el-input>
            </el-form-item>

            <el-form-item label="答案" :label-width="formLabelWidth">
              <el-input v-model="form.answer" autocomplete="off"></el-input>
            </el-form-item>
          </el-form>
          <div slot="footer" class="dialog-footer">
            <el-button @click="dialogFormVisible = false">取 消</el-button>
            <el-button type="primary" @click="handleSubmit">确 定</el-button>
          </div>
        </el-dialog>
      </el-main>
    </el-container>
  </el-container>
</template>

<script>
export default {
  name: "QuestionView",
  data() {
    return {
      formInline: {
        question: ""
      },
      currentPage: 1,
      pageSize: 4,
      total: 400,
      dialogFormVisible: false,
      formLabelWidth: "80px",
      form: {
        id: null,
        question: "",
        optiona: "",
        optionb: "",
        optionc: "",
        optiond: "",
        answer: ""
      },
      tableData: [
        { id: "1", question: "法国的首都是哪个城市？", options: "巴黎，巴黎，巴黎，巴黎", answer: "巴黎" },
        { id: "2", question: "法国的首都是哪个城市？", options: "巴黎，巴黎，巴黎，巴黎", answer: "巴黎" },
        { id: "3", question: "法国的首都是哪个城市？", options: "巴黎，巴黎，巴黎，巴黎", answer: "巴黎" },
        { id: "4", question: "法国的首都是哪个城市？", options: "巴黎，巴黎，巴黎，巴黎", answer: "巴黎" }
      ]
    };
  },
  computed: {},
  methods: {
    handleSearch() {
      this.currentPage = 1;
    },
    handleEdit() {
      // 保留弹窗功能，但不回显该行内容
      this.form = {
        id: null,
        question: "",
        optiona: "",
        optionb: "",
        optionc: "",
        optiond: "",
        answer: ""
      };
      this.dialogFormVisible = true;
    },
    handleDelete() {},
    handleSubmit() {
      this.dialogFormVisible = false;
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

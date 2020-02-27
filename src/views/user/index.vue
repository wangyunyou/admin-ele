<template>
  <div>
    <div class="topBar">
      <el-button type="primary">
        添加
        <i class="el-icon-circle-plus-outline" />
      </el-button>
      <div class="inputBox">
        <el-input
          v-model="input"
          placeholder="请输入内容"
          style="margin-right:10px"
        />
        <el-button type="success">查询</el-button>
        <el-button type="danger">清空</el-button>
      </div>
    </div>
    <el-table
      :data="tableData"
      style="width: 100%;height:630px"
      max-height="635"
      :cell-style="cellStyle"
      :header-cell-style="cellStyle"
    >
      <!-- <el-table-column label="日期" width="280">
        <template slot-scope="scope">
          <i class="el-icon-time"></i>
          <span style="margin-left: 10px">{{ scope.row.date }}</span>
        </template>
      </el-table-column>-->

      <el-table-column prop="id" label="序号" width="100" />
      <el-table-column prop="name" label="姓名" />
      <el-table-column prop="tel" label="电话" />
      <el-table-column prop="type" label="角色" />
      <el-table-column prop="workPlace" label="工作场所" />
      <el-table-column prop="status" label="状态" />

      <!-- <el-table-column label="姓名" width="280">
        <template slot-scope="scope">
          <el-popover trigger="hover" placement="top">
            <p>姓名: {{ scope.row.name }}</p>
            <p>住址: {{ scope.row.address }}</p>
            <div slot="reference" class="name-wrapper">
              <el-tag size="medium">{{ scope.row.name }}</el-tag>
            </div>
          </el-popover>
        </template>
      </el-table-column>-->

      <el-table-column label="操作">
        <template slot-scope="scope">
          <el-button size="mini" @click="handleEdit(scope.$index, scope.row)">
            编辑
          </el-button>
          <el-button
            size="mini"
            type="danger"
            @click="handleDelete(scope.$index, tableData)"
          >删除
          </el-button>
        </template>
      </el-table-column>
    </el-table>

    <div class="block">
      <el-pagination
        :current-page="currentPage4"
        :page-sizes="[10, 20, 30]"
        :page-size="10"
        layout="total, sizes, prev, pager, next, jumper"
        :total="100"
        @size-change="handleSizeChange"
        @current-change="handleCurrentChange"
      />
    </div>

    <el-dialog title="编辑信息" :visible.sync="dialogFormVisible">
      <el-form :model="form">
        <el-form-item label="姓名" :label-width="formLabelWidth">
          <el-input v-model="form.name" autocomplete="off" />
        </el-form-item>
        <el-form-item label="电话" :label-width="formLabelWidth">
          <el-input v-model="form.tel" autocomplete="off" />
        </el-form-item>
        <el-form-item label="角色" :label-width="formLabelWidth">
          <el-checkbox label="理货员" name="type" />
          <el-checkbox label="配送员" name="type" />
        </el-form-item>
        <el-form-item label="工作场所" :label-width="formLabelWidth">
          <el-select v-model="form.workPlace" placeholder="请选择活动区域">
            <el-option label="理货中心A" value="01" />
            <el-option label="理货中心B" value="02" />
          </el-select>
        </el-form-item>
        <el-form-item label="用户状态" :label-width="formLabelWidth">
          <el-select v-model="form.state" placeholder="请选择用户状态">
            <el-option label="正常" value="01" />
            <el-option label="离职" value="02" />
          </el-select>
        </el-form-item>
        <el-form-item label="登录密码" :label-width="formLabelWidth">
          <el-input
            v-model="form.pwd"
            autocomplete="off"
            show-password
          />
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="dialogFormVisible = false">取 消</el-button>
        <el-button
          type="primary"
          @click="dialogFormVisible = false"
        >保 存</el-button>
      </div>
    </el-dialog>
  </div>
</template>

<script>
export default {
  data() {
    return {
      input: '',
      tableData: [],
      dialogTableVisible: false,
      dialogFormVisible: false,
      form: {
        name: '',
        region: '',
        date1: '',
        date2: '',
        delivery: false,
        type: [],
        resource: '',
        desc: ''
      },
      formLabelWidth: '120px',
      currentPage4: 1
    }
  },
  mounted() {
    this.run()
  },
  methods: {
    handleEdit(index, row) {
      this.dialogFormVisible = true
      console.log(index, row)
      this.form.name = row.name
      this.form.tel = row.tel
    },
    handleDelete(index, row) {
      this.$confirm('此操作将永久删除该文件, 是否继续?', '提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
      })
        .then(() => {
          row.splice(index, 1)
          this.$message({
            type: 'success',
            message: '删除成功!'
          })
        })
        .catch(() => {
          this.$message({
            type: 'info',
            message: '已取消删除'
          })
        })
    },
    handleSizeChange(val) {
      console.log(`每页 ${val} 条`)
    },
    handleCurrentChange(val) {
      console.log(`当前页: ${val}`)
    },
    cellStyle() {
      return 'text-align:center'
    },
    run() {
      const that = this
      const axios = require('axios')
      axios
        .get(
          'http://rest.apizza.net/mock/480fbdd9fc2eaff20db72dd45597fbc2/test-wyy.json'
        )
        .then(function(e) {
          // console.log(e.data.data);
          that.tableData = e.data.data
        })
        .catch(function(res) {
          // console.log(res);
        })
    }
  }
}
</script>

<style scoped>
.topBar {
  width: 100%;
  height: 65px;
  border-top: 5px solid #333;
  display: flex;
  justify-content: space-between;
  padding: 10px;
  box-sizing: border-box;
  background-color: #fff;
}

.inputBox {
  display: flex;
}

.el-input,
.el-select {
  width: 90%;
}

.block {
  width: 100%;
  height: 60px;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: #fff;
}
</style>

<template>
  <div class="admin-container">
    <div class="filter-container center">
      <div>
        <span>项目名称：</span>
        <el-input
          v-model="searchUser"
          style="width: 200px;margin-right:30px"
          @keyup.enter.native="handleFilter"
        />
        <el-button type="primary" icon="el-icon-search" @click="handleFilter">
          搜索
        </el-button>
      </div>
      <el-button type="primary" icon="el-icon-edit" @click="handleCreate">
        添加
      </el-button>
    </div>
    <el-row :gutter="20" v-if="userList.length != 0">
      <el-col :span="8" v-for="(user, index) in userList" :key="index">
        <el-card class="box-card" shadow="hover">
          <div
            slot="header"
            class="center"
            style="font-weight:bold;font-size:18px"
          >
            <span>{{ user.title }}</span>
            <div>
              <el-button size="small" @click="handleUpdate(user)" type="primary"
                >编辑</el-button
              >
              <el-button size="small" @click="handleDelete(user)" type="danger"
                >删除</el-button
              >
              <el-button size="small" @click="handleInfo(user)" type="info"
                >查看详情</el-button
              >
            </div>
          </div>
          <div>
            <div class="card-content-item">
              <span class="content-title">负责人：</span
              ><span class="content">{{ user.charge }}</span>
            </div>
            <div class="card-content-item">
              <span class="content-title">联系方式：</span
              ><span class="content">{{ user.contact }}</span>
            </div>
            <div class="card-content-item">
              <span class="content-title">用户名：</span
              ><span class="content">{{ user.user }}</span>
            </div>
            <div class="card-content-item info">
              <span class="content-title">简介：</span
              ><span class="content">{{ user.info }}</span>
            </div>
          </div>
        </el-card></el-col
      >
    </el-row>
    <div v-else class="empty-container">
      暂无数据
    </div>
    <el-dialog :title="textMap[dialogStatus]" :visible.sync="dialogFormVisible">
      <el-form
        ref="dataForm"
        :rules="rules"
        :model="temp"
        label-position="left"
        label-width="90px"
        style="width: 400px; margin-left:50px;"
      >
        <el-form-item label="项目名称" prop="title">
          <el-input v-model.trim="temp.title" />
        </el-form-item>
        <el-form-item label="负责人" prop="charge">
          <el-input v-model.trim="temp.charge" />
        </el-form-item>
        <el-form-item label="联系方式" prop="contact">
          <el-input v-model.trim="temp.contact" />
        </el-form-item>
        <el-form-item label="用户名" prop="user">
          <el-input v-model.trim="temp.user" />
        </el-form-item>
        <el-form-item label="密码" prop="password">
          <el-input v-model.trim="temp.password" show-password />
        </el-form-item>
        <el-form-item label="确认密码" prop="password_confirm">
          <el-input v-model.trim="temp.password_confirm" show-password />
        </el-form-item>
        <el-form-item label="项目简介" prop="info">
          <el-input
            v-model="temp.info"
            :autosize="{ minRows: 2, maxRows: 4 }"
            type="textarea"
          />
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="dialogFormVisible = false">
          取消
        </el-button>
        <el-button
          type="primary"
          @click="dialogStatus === 'create' ? createData() : updateData()"
        >
          确定
        </el-button>
      </div>
    </el-dialog>
    <el-dialog title="提示" :visible.sync="dialogDeleteVisible">
      <div>确认删除项目？</div>
      <div slot="footer" class="dialog-footer">
        <el-button @click="dialogDeleteVisible = false">
          取消
        </el-button>
        <el-button type="primary" @click="deleteUser">
          确定
        </el-button>
      </div>
    </el-dialog>
  </div>
</template>

<script>
export default {
  data() {
    var validatePass = (rule, value, callback) => {
      if (value === "") {
        callback(new Error("请再次输入密码"));
      } else if (value !== this.temp.password) {
        callback(new Error("两次输入密码不一致!"));
      } else {
        callback();
      }
    };
    return {
      searchUser: "",
      allUser: [
        {
          title: "项目1",
          charge: "张小明",
          contact: "13953516345",
          user: "test",
          info:
            "一段文字本项目是上海市电力公司关于施工单位的项目，最多两行一段文字本项目是上海市电力公司关于施工单位的项目，最多两行一段文字本项目是上海市电力公司关于施工单位的项目，最多两行"
        },
        {
          title: "项目2",
          charge: "张小明",
          contact: "13953516345",
          user: "test",
          info: "一段文字"
        },
        {
          title: "项目3",
          charge: "张小明",
          contact: "13953516345",
          user: "test",
          info: "一段文字"
        },
        {
          title: "项目4",
          charge: "张小明",
          contact: "13953516345",
          user: "test",
          info: "一段文字"
        },
        {
          title: "项目5",
          charge: "张小明",
          contact: "13953516345",
          user: "test",
          info: "一段文字"
        }
      ],
      userList: null,
      temp: {
        id: undefined,
        time: "",
        remark: "",
        timestamp: new Date(),
        title: "test",
        queryData: "",
        form: ""
      },
      dialogFormVisible: false,
      dialogDeleteVisible: false,
      deleteTemp: null,
      dialogStatus: "",
      textMap: {
        update: "修改",
        create: "添加"
      },
      temp: {
        title: "",
        charge: "",
        contact: "",
        user: "",
        password: "",
        password_confirm: "",
        info: ""
      },
      triggerOptions: [
        { label: "数据驱动", key: "data" },
        { label: "时间驱动", key: "time" }
      ],
      rules: {
        title: [{ required: true, message: "名称必填", trigger: "blur" }],
        charge: [{ required: true, message: "联系人必填", trigger: "blur" }],
        contact: [{ required: true, message: "联系方式必填", trigger: "blur" }],
        user: [{ required: true, message: "用户名必填", trigger: "blur" }],
        password: [{ required: true, message: "密码必填", trigger: "blur" }],
        password_confirm: [
          { validator: validatePass, trigger: "blur" },
          { required: true, message: "密码必填", trigger: "blur" }
        ],
        info: [{ required: true, message: "项目简介必填", trigger: "blur" }]
      }
    };
  },
  created() {
    this.userList = this.allUser;
  },
  methods: {
    handleDelete(user) {
      this.dialogDeleteVisible = true;
      this.deleteTemp = user;
    },
    deleteUser() {
      this.dialogDeleteVisible = false;
      console.log("this.deleteTemp: ", this.deleteTemp);
    },
    handleFilter() {
      this.userList = this.allUser.filter(user => {
        return user.title.indexOf(this.searchUser.trim()) != -1;
      });
    },
    handleInfo(user) {
      console.log("user: ", user);
      this.$router.push("/userInfo");
    },
    resetTemp() {
      this.temp = {
        id: undefined,
        importance: 1,
        remark: "",
        timestamp: new Date(),
        title: "",
        trigger: "data",
        type: ""
      };
    },
    handleCreate() {
      this.resetTemp();
      this.dialogStatus = "create";
      this.dialogFormVisible = true;
      this.$nextTick(() => {
        this.$refs["dataForm"].clearValidate();
      });
    },
    handleUpdate(user) {
      this.dialogStatus = "update";
      this.dialogFormVisible = true;
      this.$nextTick(() => {
        this.$refs["dataForm"].clearValidate();
      });
    }
  }
};
</script>

<style lang="scss" scoped>
.admin-container {
  padding: 15px;
  .filter-container {
    height: 80px;
    line-height: 80px;
  }
  .empty-container {
    text-align: center;
    color: gray;
    margin-top: 30px;
  }

  .box-card {
    margin-bottom: 15px;
    .card-content-item {
      line-height: 30px;
      .content-title {
        display: inline-block;
        width: 90px;
        color: grey;
      }
      .content {
        display: inline-block;
      }
    }
    .info {
      display: flex;
      // justify-content: space-between;
      .content {
        overflow: hidden;
        text-overflow: ellipsis;
        display: -webkit-box;
        -webkit-line-clamp: 2;
        -webkit-box-orient: vertical;
        height: 60px;
        width: calc(100% - 90px);
      }
    }
  }
}
</style>

<template>
  <div class="app-container">
    <el-form ref="form" :model="project" label-width="80px" :rules="rules">
      <el-form-item label="用户名" prop="user">
        <el-input v-model="project.user" class="input-item"></el-input>
        <el-button type="primary">修改</el-button>
      </el-form-item>

      <el-form-item label="密码" prop="password">
        <el-input v-model="project.password" class="input-item" show-password>
        </el-input>
      </el-form-item>
      <el-form-item label="确认密码" prop="password_confirm">
        <el-input v-model="project.password" class="input-item" show-password>
        </el-input>
        <el-button type="primary">修改</el-button>
      </el-form-item>
    </el-form>
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
      project: {
        user: "",
        password: "",
        password_confirm: ""
      },
      rules: {
        user: [{ required: true, message: "名称必填", trigger: "blur" }],
        password: [{ required: true, message: "密码必填", trigger: "blur" }],
        password_confirm: [
          { validator: validatePass, trigger: "blur" },
          { required: true, message: "密码必填", trigger: "blur" }
        ]
      }
    };
  }
};
</script>
<style lang="scss" scoped>
.input-item {
  width: 400px;
  margin-right: 30px;
}
</style>

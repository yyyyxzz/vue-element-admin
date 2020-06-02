<template>
  <div class="admin-container">
    <div class="filter-container">
      <span>用户名称：</span>
      <el-input
        v-model="searchUser"
        style="width: 200px;margin-right:30px"
        @keyup.enter.native="handleFilter"
      />
      <el-button type="primary" @click="handleFilter">查询</el-button>
    </div>
    <el-row :gutter="20" v-if="userList.length != 0">
      <el-col :span="12" v-for="(user, index) in userList" :key="index">
        <el-card class="box-card">
          <div slot="header" class="clearfix">
            <span>{{ user.name }}</span>
            <el-button style="float: right; padding: 3px 0" type="text" @click="handleInfo(user)"
              >查看详情</el-button
            >
          </div>
          <div v-for="o in 4" :key="o" class="text item">
            {{ "列表内容 " + o }}
          </div>
        </el-card></el-col
      >
    </el-row>
    <div v-else class="empty-container">
      暂无数据
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      searchUser: "",
      allUser: [
        { name: "user1" },
        { name: "user2" },
        { name: "user3" },
        { name: "user4" },
        { name: "user5" }
      ],
      userList: null
    };
  },
  created() {
    this.userList = this.allUser;
  },
  methods: {
    handleFilter() {
      this.userList = this.allUser.filter(user => {
        return user.name.indexOf(this.searchUser.trim()) != -1;
      });
    },
    handleInfo(user){
      console.log('user: ', user);
      this.$router.push('/userInfo')
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
  }
}
</style>

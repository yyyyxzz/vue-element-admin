<template>
  <div class="app-container">
    <el-card class="box-card" style="margin-bottom:30px">
      <div slot="header" class="clearfix">
        <span class="title">数据项</span>
      </div>
      <data-item></data-item>
    </el-card>
    <el-card class="box-card">
      <div slot="header" class="clearfix">
        <span class="title">动态</span>
      </div>
      <record></record>
    </el-card>
  </div>
</template>

<script>
import dataItem from "./data-item";
import record from "./record";

export default {
  data() {
    return {};
  },
  components: { dataItem, record }
};
</script>
<style lang="scss" scoped>
.box-card{
  .title{
    font-size:20px;
    font-weight: bold;
  }
}
</style>

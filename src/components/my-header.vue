<template>
  <el-row :gutter="20">
    <el-col :span="4" style="text-align: center"><span><b>Account Platform</b></span></el-col>
    <el-col :span="20" style="text-align: right">
      <el-dropdown>
        <i class="el-icon-setting" style="margin-right: 15px"></i>
        <el-dropdown-menu slot="dropdown">
          <el-dropdown-item>查看</el-dropdown-item>
          <el-dropdown-item>新增</el-dropdown-item>
          <el-dropdown-item>删除</el-dropdown-item>
        </el-dropdown-menu>
      </el-dropdown>
      <span>会灰翔的灰机</span>
    </el-col>
  </el-row>
</template>

<script>
  export default {
    name: "my-header"
  }
</script>

<style scoped>

</style>
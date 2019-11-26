<template>
  <el-container style="height: 500px; border: 1px solid #eee">
    <el-header style="font-size: 12px">
      <myHeader></myHeader>
    </el-header>
    <el-container>
      <el-aside width="200px" style="background-color: rgb(238, 241, 246)">
        <myAside ref="myAside" v-on:selectItem="selectItemFromChild"></myAside>
      </el-aside>
      <el-main>
        <myMain ref="myMain" v-on:removeTab="removeTabFromChild"></myMain>
      </el-main>
    </el-container>
  </el-container>
</template>

<script>
  import myHeader from '../components/my-header'
  import myAside from '../components/my-aside'
  import myMain from '../components/my-main'

  export default {
    components: {
      myHeader,
      myAside,
      myMain
    },
    methods: {
      selectItemFromChild(tab) {
        console.log("selectItemFromChild:"+tab);
        this.$refs.myMain.selectItemFromParent(tab);
      },
      removeTabFromChild(tab) {
        console.log("removeTabFromChild:"+tab);
        this.$refs.myAside.selectItem(tab);
      }
    }
  };
</script>

<style>
  .el-header {
    color: #333;
    line-height: 60px;
  }

  .el-aside {
    color: #333;
  }

  a {
    text-decoration: none;
  }

  .router-link-active {
    text-decoration: none;
  }
</style>
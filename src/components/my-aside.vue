<template>
  <el-menu :default-active="menuItemActiveIndex">
    <div v-for="(menuItem, menuItemIndex) in menuItems" :key="menuItemIndex">
      <router-link :to="menuItem.path">
        <el-menu-item :index="String(menuItemIndex)">
          <i class="el-icon-menu"></i>
          <span slot="title" @click="addTab(menuItem, menuItemIndex)">{{menuItem.title}}</span>
        </el-menu-item>
      </router-link>
    </div>
  </el-menu>
</template>

<script>
  export default {
    name: "my-aside",
    data() {
      return {
        menuItems: [{
          title: '收支表',
          name: 'fund_order',
          path: '/'
        },{
          title: '收支类型表',
          name: 'fund_order_type',
          path: '/fund_order_type'
        },{
          title: '账户信息表',
          name: 'account_info',
          path: '/account_info'
        },{
          title: '产品信息表',
          name: 'product_info',
          path: '/product_info'
        }],
        menuItemActiveIndex: '0'
      }
    },
    mounted() {
      this.init()
    },
    methods: {
      addTab(targetItem, index) {
        console.log("index=" + index);
        this.menuItemActiveIndex = String(index);
        this.$emit('addTab', targetItem);
      },
      init() {
        this.addTab(this.menuItems[0]);
        // 不知道为啥，必须在这里初始化默认值才能使第一个选项卡被默认选中，直接data中默认值为'0'是不生效的
        this.menuItemActiveIndex = String(0);
      }
    }
  }
</script>

<style scoped>

</style>
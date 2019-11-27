<template>
  <el-menu :default-active="activeMenuItemName">
    <div v-for="(menuItem) in menuItems" :key="menuItem.index">
      <router-link :to="menuItem.path">
        <el-menu-item :index="menuItem.name">
          <i class="el-icon-menu"></i>
          <span slot="title" @click="selectItem(menuItem)">{{menuItem.title}}</span>
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
          path: '/fund_order',
          componentName: "fundOrder"
        },{
          title: '收支类型表',
          name: 'fund_order_type',
          path: '/fund_order_type',
          componentName: "fundOrderType"
        },{
          title: '账户信息表',
          name: 'account_info',
          path: '/account_info',
          componentName: "accountInfo"
        },{
          title: '产品信息表',
          name: 'product_info',
          path: '/product_info',
          componentName: "productInfo"
        }],
        activeMenuItemName: 'fund_order'
      }
    },
    mounted() {
      this.init()
    },
    methods: {
      selectItem(targetItem) {
        if (targetItem == null) {
          console.log("删除了最后一个tab不展示");
          this.activeMenuItemName = null;
          return;
        }
        this.activeMenuItemName = targetItem.name;
        this.$emit('selectItem', targetItem);
      },
      init() {
        this.selectItem(this.menuItems[0]);
      }
    }
  }
</script>

<style scoped>

</style>
<template>
  <div>
    <div style="margin-bottom: 1px;">
    </div>
    <el-tabs v-model="editableTabsValue" :active-name="editableTabsValue" type="card" closable @tab-remove="removeTab" v-on:tab-click="tabClick">
      <el-tab-pane
          v-for="(item) in editableTabs"
          :key="item.name"
          :label="item.title"
          :name="item.name"
      >
      </el-tab-pane>
    </el-tabs>
    <component v-bind:is="currentTabComponent"></component>
  </div>
</template>

<script>
  import fundOrder from '../components/fund-order'
  import fundOrderType from '../components/fund-order-type'
  import accountInfo from '../components/account-info'
  import productInfo from '../components/product-info'

  export default {
    name: "my-main",
    data() {
      return {
        editableTabsValue: '',
        editableTabs: [],
        currentTabComponent: "fundOrder"
      };
    },
    components: {
      fundOrder,
      fundOrderType,
      accountInfo,
      productInfo
    },
    methods: {
      selectItemFromParent(tab) {
        console.log(tab);
        this.editableTabsValue = tab.name;
        this.currentTabComponent = tab.componentName;
        let exist = false;
        this.editableTabs.forEach((editableTab) => {
          if (editableTab.name === tab.name) {
            exist = true;
          }
        });
        if (exist) {
          return;
        }
        this.editableTabs.push(tab);
      },
      removeTab(targetName) {
        let tabs = this.editableTabs;
        let activeName = this.editableTabsValue;
        let activeTab = null;
        if (activeName === targetName) {
          tabs.forEach((tab, index) => {
            if (tab.name === targetName) {
              let nextTab = tabs[index + 1] || tabs[index - 1];
              if (nextTab) {
                activeName = nextTab.name;
                activeTab = nextTab
                this.$router.push(activeTab.path);
              }
            }
          });
        }
        this.editableTabsValue = activeName;
        this.editableTabs = tabs.filter(tab => tab.name !== targetName);
        if (activeTab == null) {
          this.editableTabs.forEach((tab) => {
            if (tab.name === this.editableTabsValue) {
              console.log("found tab:"+tab.name+","+tab.path);
              activeTab = tab
            }
          });
        }
        console.log("targetName:"+targetName+",activeName:"+activeName+",activeTab:"+activeTab);
        this.$emit("returnActiveTab", activeTab);
      },
      tabClick(tab, event) {
        console.log("activeName:"+tab.name+",event:"+event+",tab.path:"+tab.path);
        let activeTab;
        this.editableTabs.forEach((tab) => {
          if (tab.name === this.editableTabsValue) {
            console.log("found tab:"+tab.name+","+tab.path);
            activeTab = tab
            this.$router.push(activeTab.path);
          }
        });
        this.$emit("returnActiveTab", activeTab);
      }
    }
  }
</script>

<style scoped>

</style>
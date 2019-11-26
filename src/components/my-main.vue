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
    <el-row :gutter="20">
      <el-col :span="12">
        <el-select
            v-model="value"
            multiple
            filterable
            default-first-option
            placeholder="请选择标签">
          <el-option
              v-for="item in options"
              :key="item.value"
              :label="item.label"
              :value="item.value">
          </el-option>
        </el-select>
      </el-col>
      <el-col :span="12" style="text-align: right;">
        <el-button icon="el-icon-search" circle></el-button>
        <el-button type="danger" icon="el-icon-delete" circle style="margin-right: 20px;"></el-button>
      </el-col>
    </el-row>
    <el-table
        ref="multipleTable"
        :data="tableData"
        tooltip-effect="dark"
        height="380"
        border
        style="width: 100%; margin-bottom: 10px; margin-top: 15px;"
        :default-sort = "{prop: 'date', order: 'descending'}"
        @selection-change="handleSelectionChange">
      <el-table-column
          type="selection"
          width="55">
      </el-table-column>
      <el-table-column
          fixed
          label="日期"
          width="150"
          sortable>
        <template slot-scope="scope">{{ scope.row.date }}</template>
      </el-table-column>
      <el-table-column
          prop="account_name"
          label="账户"
          width="120"
          sortable>
      </el-table-column>
      <el-table-column
          prop="product_name"
          label="产品"
          width="120">
      </el-table-column>
      <el-table-column
          prop="account_type"
          label="账户类型"
          width="120">
      </el-table-column>
      <el-table-column
          prop="payment"
          label="收支"
          width="300"
          show-overflow-tooltip>
      </el-table-column>
      <el-table-column
          fixed="right"
          label="操作"
          width="120">
        <template slot-scope="scope">
          <el-button @click="queryDetail(scope.row)" type="text" size="small">查看</el-button>
          <el-button type="text" size="small">编辑</el-button>
          <el-button type="text" size="small">删除</el-button>
        </template>
      </el-table-column>
    </el-table>
    <el-pagination
        background
        layout="prev, pager, next"
        :total="totalCount" style="text-align:right">
    </el-pagination>
  </div>
</template>

<script>
  export default {
    name: "my-main",
    data() {
      return {
        tableData: [],
        editableTabsValue: '',
        multipleSelection: [],
        options: [{
          value: '1',
          label: 'HTML'
        }, {
          value: '2',
          label: 'CSS'
        }, {
          value: '3',
          label: 'JavaScript'
        }],
        value: [],
        editableTabs: []
      };
    },
    computed: {
      totalCount() {
        return this.tableData.length;
      }
    },
    mounted() {
      this.init();
    },
    methods: {
      selectItemFromParent(tab) {
        console.log(tab);
        this.editableTabsValue = tab.name;
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
      },
      queryDetail(row) {
        console.log(row);
      },
      toggleSelection(rows) {
        if (rows) {
          rows.forEach(row => {
            this.$refs.multipleTable.toggleRowSelection(row);
          });
        } else {
          this.$refs.multipleTable.clearSelection();
        }
      },
      handleSelectionChange(val) {
        this.multipleSelection = val;
        console.log(this.multipleSelection);
      },
      init() {
        this.tableData = [{},{},{},{},{},{},{},{},{},{},{},{},{},{},{},{},{},{},{},{},{},{},{},{},{}];
      }
    }
  }
</script>

<style scoped>

</style>
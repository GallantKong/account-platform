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
    <el-row :gutter="50">
      <el-col :xs="9" :sm="9" :md="9" :lg="9" :xl="9">
        <div class="block">
          <el-date-picker
              v-model="value2"
              type="daterange"
              align="right"
              unlink-panels
              range-separator="至"
              start-placeholder="开始日期"
              end-placeholder="结束日期"
              :picker-options="pickerOptions">
          </el-date-picker>
        </div>
      </el-col>
      <el-col :xs="11" :sm="11" :md="11" :lg="11" :xl="11">
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
      <el-col :xs="1" :sm="1" :md="1" :lg="1" :xl="1">
        <el-button icon="el-icon-search" circle></el-button>
      </el-col>
      <el-col :xs="1" :sm="1" :md="1" :lg="1" :xl="1">
        <el-button type="danger" icon="el-icon-delete" circle style="margin-right: 20px;"></el-button>
      </el-col>
    </el-row>
    <el-row :gutter="20">
      <el-button type="primary" style="margin-left: 20px;margin-top: 10px">新增</el-button>
    </el-row>
    <el-table
        ref="multipleTable"
        :data="tableData"
        tooltip-effect="dark"
        height="380"
        border
        style="width: 100%; margin-bottom: 10px; margin-top: 10px;"
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
          width="300">
      </el-table-column>
      <el-table-column
          prop="account_type"
          label="账户类型"
          width="120">
      </el-table-column>
      <el-table-column
          prop="payment"
          label="收支"
          width="120"
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
          label: '信用卡'
        }, {
          value: '2',
          label: '储蓄卡'
        }, {
          value: '3',
          label: '花呗'
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
<template>
  <el-container style="height: 500px; border: 1px solid #eee">
    <el-header style="font-size: 12px">
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
    </el-header>
    <el-container>
      <el-aside width="200px" style="background-color: rgb(238, 241, 246)">
        <el-menu :default-active="menuItemActiveIndex" v-for="(menuItem, menuItemIndex) in menuItems" :key="menuItemIndex">
          <router-link :to="menuItem.path">
          <el-menu-item :index="menuItemIndex">
            <i class="el-icon-menu"></i>
            <span slot="title" @click="addTab(menuItem, menuItemIndex)">{{menuItem.title}}</span>
          </el-menu-item>
          </router-link>
        </el-menu>
      </el-aside>
      <el-main>
        <div style="margin-bottom: 1px;">
        </div>
        <el-tabs v-model="editableTabsValue" type="card" closable @tab-remove="removeTab">
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
      </el-main>
    </el-container>
  </el-container>
</template>

<script>
  export default {
    data() {
      return {
        tableData: [],
        editableTabsValue: '1',
        menuItems: [{
          title: '收支表',
          name: 'fund_order',
          path: '/'
        },{
          title: '收支类型表',
          name: 'fund_type',
          path: '/fund_type'
        },{
          title: '账户信息表',
          name: 'account_info',
          path: '/account_info'
      },{
          title: '产品信息表',
          name: 'product_info',
          path: '/product_info'
      }],
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
        editableTabs: [],
        menuItemActiveIndex: 0
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
      addTab(targetItem, index) {
        let targetName = targetItem.title;
        console.log("index=" + index);
        this.menuItemActiveIndex = index;
        let exist = false;
        this.editableTabs.forEach((editableTab) => {
              if (editableTab.title === targetName) {
                exist = true;
              }
            }
        );
        if (exist) {
          return;
        }
        this.editableTabs.push({
          title: targetName,
          name: targetName,
          content: 'New Tab content'
        });
        this.editableTabsValue = targetName;
      },
      removeTab(targetName) {
        let tabs = this.editableTabs;
        let activeName = this.editableTabsValue;
        if (activeName === targetName) {
          tabs.forEach((tab, index) => {
            if (tab.name === targetName) {
              let nextTab = tabs[index + 1] || tabs[index - 1];
              if (nextTab) {
                activeName = nextTab.name;
              }
            }
          });
        }

        this.editableTabsValue = activeName;
        this.editableTabs = tabs.filter(tab => tab.name !== targetName);
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
        this.editableTabs.push(this.menuItems[0])
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

  .router-link-active {
    text-decoration: none;
  }
</style>
<template>
  <div>
    <el-row :gutter="50">
      <el-col :xs="9" :sm="9" :md="9" :lg="9" :xl="9">
        <div class="block">
          <el-date-picker
              v-model="dateRangeValue"
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
            v-model="selectedOptionValues"
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
        <el-button icon="el-icon-search" circle @click="searchClick"></el-button>
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
        :total="totalCount" style="text-align:right" @current-change="currentPageChange" :page-size="pageSize">
    </el-pagination>
  </div>
</template>

<script>
  import request from '../utils/request'
  export default {
    name: "fund-order",
    data() {
      return {
        tableData: [],
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
        selectedOptionValues: [],
        pickerOptions: {
          shortcuts: [{
            text: '最近一周',
            onClick(picker) {
              const end = new Date();
              const start = new Date();
              start.setTime(start.getTime() - 3600 * 1000 * 24 * 7);
              picker.$emit('pick', [start, end]);
            }
          }, {
            text: '最近一个月',
            onClick(picker) {
              const end = new Date();
              const start = new Date();
              start.setTime(start.getTime() - 3600 * 1000 * 24 * 30);
              picker.$emit('pick', [start, end]);
            }
          }, {
            text: '最近三个月',
            onClick(picker) {
              const end = new Date();
              const start = new Date();
              start.setTime(start.getTime() - 3600 * 1000 * 24 * 90);
              picker.$emit('pick', [start, end]);
            }
          }]
        },
        dateRangeValue: '',
        pageSize: 10
      }
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
        // this.tableData = [{},{},{},{},{},{},{},{},{},{},{},{},{},{},{},{},{},{},{},{},{},{},{},{},{}].splice(0, this.pageSize);
        this.tableData = [{},{},{},{},{},{},{},{},{},{},{},{},{},{},{},{},{},{},{},{},{},{},{},{},{}];
      },
      searchClick(event) {
        let that = this;
        console.log("searchClick.event:"+event+",selectedOptionValues:"+ that.selectedOptionValues + ",dataRange:"+ that.dateRangeValue);
        request({
              method:"get",
              url:"/fund/order",
              params:{}
            }).then((response)=>{
                      console.log("response "+response);
                  })
      },
      currentPageChange(currentPage) {
        console.log("currentPage:" + currentPage);
      }
    }
  }
</script>

<style scoped>

</style>
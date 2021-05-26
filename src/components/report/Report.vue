<template>
  <div>
    <el-breadcrumb separator-class="el-icon-arrow-right">
      <el-breadcrumb-item :to="{ path: '/Home' }">Home</el-breadcrumb-item>
      <el-breadcrumb-item>Statistics</el-breadcrumb-item>
      <el-breadcrumb-item>Report</el-breadcrumb-item>
    </el-breadcrumb>

    <el-card>
      <div id="main" style="width: 750px; height: 400px"></div>
    </el-card>
  </div>
</template>

<script>
// 1. 导入 echarts
import * as echarts from "echarts";
import _ from "lodash";

export default {
  data() {
    return {
      // 需要合并的数据
      options: {
        title: {
          text: "User Source",
        },
        tooltip: {
          trigger: "axis",
          axisPointer: {
            type: "cross",
            label: {
              backgroundColor: "#E9EEF3",
            },
          },
        },
        grid: {
          left: "3%",
          right: "4%",
          bottom: "3%",
          containLabel: true,
        },
        xAxis: [
          {
            boundaryGap: false,
          },
        ],
        yAxis: [
          {
            type: "value",
          },
        ],
      },
    };
  },
  created() {},
  async mounted() {
    // let charts = require("echarts");
    var myChart = echarts.init(document.getElementById("main"));
    const { data: res } = await this.$http.get("reports/type/1");
    if (res.meta.status !== 200) {
      return this.$message.error("Fail to get chart");
    }
    console.log("aaaa");
    const result = _.merge(res.data, this.options);
    myChart.setOption(result);
  },
  methods: {},
};
</script>

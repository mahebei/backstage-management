<template>
  <div>
    <el-breadcrumb separator-class="el-icon-arrow-right">
      <el-breadcrumb-item :to="{ path: '/Home' }">Home</el-breadcrumb-item>
      <el-breadcrumb-item>Order Management</el-breadcrumb-item>
      <el-breadcrumb-item>Order List</el-breadcrumb-item>
    </el-breadcrumb>

    <el-card>
      <el-row>
        <el-col :span="8">
          <el-input placeholder="Please Enter">
            <el-button slot="append" icon="el-icon-search"></el-button>
          </el-input>
        </el-col>
        <el-col></el-col>
      </el-row>

      <el-table :data="orderList" border stripe>
        <el-table-column type="index"></el-table-column>
        <el-table-column label="Order No" prop="order_number"></el-table-column>
        <el-table-column
          label="Order Price"
          prop="order_price"
        ></el-table-column>
        <el-table-column label="Payment Status" prop="pay_status">
          <template slot-scope="scope">
            <el-tag type="success" v-if="scope.row.pay_status === '1'"
              >Paid</el-tag
            >
            <el-tag type="danger" v-else>Not Paid</el-tag>
          </template>
        </el-table-column>
        <el-table-column label="Shipment Status" prop="is_send">
          <template slot-scope="scope">
            <el-tag type="success" v-if="scope.row.is_send === '是'"
              >Shipped</el-tag
            >
            <el-tag type="danger" v-else>Not Shipped</el-tag>
          </template>
        </el-table-column>
        <el-table-column label="Order Time" prop="create_time">
          <template slot-scope="scope">
            {{ scope.row.create_time | dateFormat }}
          </template>
        </el-table-column>
      </el-table>

      <el-pagination
        @size-change="handleSizeChange"
        @current-change="handleCurrentChange"
        :current-page="queryInfo.pagenum"
        :page-sizes="[5, 10, 15]"
        :page-size="queryInfo.pagesize"
        layout="total, sizes, prev, pager, next, jumper"
        :total="total"
      ></el-pagination>
    </el-card>
  </div>
</template>

<script>
export default {
  data() {
    return {
      queryInfo: {
        query: "",
        pagenum: 1,
        pagesize: 10,
      },
      total: 0,
      orderList: [],
    };
  },
  created() {
    this.getOrderList();
  },
  methods: {
    async getOrderList() {
      const { data: res } = await this.$http.get("orders", {
        params: this.queryInfo,
      });
      if (res.meta.status !== 200) {
        return this.$message.error("Fail to get order list");
      }
      // console.log(res)
      this.total = res.data.total;
      this.orderList = res.data.goods;
    },
    handleSizeChange(newSize) {
      this.queryInfo.pagesize = newSize;
      this.getOrderList();
    },
    handleCurrentChange(newPage) {
      this.queryInfo.pagenum = newPage;
      this.getOrderList();
    },
  },
};
</script>

<style scoped>
</style>
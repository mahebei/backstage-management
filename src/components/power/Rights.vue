<template>
  <div>
    <el-breadcrumb separator-class="el-icon-arrow-right">
      <el-breadcrumb-item :to="{ path: '/Home' }">Home</el-breadcrumb-item>
      <el-breadcrumb-item>Rights Management</el-breadcrumb-item>
      <el-breadcrumb-item>Rights List</el-breadcrumb-item>
    </el-breadcrumb>
    <el-card>
      <el-table :data="rightsList" border stripe>
        <el-table-column type="index"></el-table-column>
        <el-table-column label="Rights Name" prop="authName"></el-table-column>
        <el-table-column label="Path" prop="path"></el-table-column>
        <el-table-column label="Rights Level" prop="level">
          <template slot-scope="scope">
            <el-tag v-if="scope.row.level === '0'">Level 1</el-tag>
            <el-tag type="success" v-else-if="scope.row.level === '1'"
              >Level 2</el-tag
            >
            <el-tag type="warning" v-else>Level 3</el-tag>
          </template>
        </el-table-column>
      </el-table>
    </el-card>
  </div>
</template>

<script>
export default {
  data() {
    return {
      rightsList: [],
    };
  },
  created() {
    this.getRightsList();
  },
  methods: {
    async getRightsList() {
      const { data: res } = await this.$http.get("rights/list");
      if (res.meta.status !== 200) {
        return this.$message.error("Fail to get rights");
      }
      this.rightsList = res.data;
      console.log(this.rightsList);
    },
  },
};
</script>

<style lang="scss" scoped>
</style>
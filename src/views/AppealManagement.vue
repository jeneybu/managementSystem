<template>
  <div>
    <el-table
      ref="multipleTable"
      :data="dataList"
      tooltip-effect="dark"
      style="width: 100%"
      @selection-change="handleSelectionChange"
    >
      <el-table-column prop="id" label="ID"></el-table-column>
      <el-table-column prop="createTime" label="时间"></el-table-column>
      <el-table-column prop="userName" label="昵称"></el-table-column>
      <el-table-column prop="statusMemo" label="状态"></el-table-column>
      <el-table-column prop="createTime" label="日期"></el-table-column>
    </el-table>
    <br />
    <Page
      :total="total"
      show-sizer
      show-elevator
      @on-change="handleCurrentPage($event)"
      @on-page-size-change="handlePageSize($event)"
    />
  </div>
</template>

<script>
export default {
  name: "ArticleManagement",
  data() {
    return {
      kindList: [
        {
          value: 1,
          label: "科幻"
        },
        {
          value: 2,
          label: "恐怖"
        }
      ],
      dataList: [],
      total: null,
      pageSize: 10,
      currentPage: 1,
      updateContent: ''
    };
  },
  created() {
    this.getOrderData();
  },
  methods: {
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
    },
    // 切换页面触发
    handleCurrentPage(val) {
      this.currentPage = val;
      this.getOrderData();
    },
    // 切换pageSize触发
    handlePageSize(val) {
      this.pageSize = val;
      this.getOrderData();
    },
    // 获取列表数据
    getOrderData() {
      var data = {
        pageSize: this.pageSize,
        pageNum: this.currentPage
      };
      this.$get("/admin/article_complaint/getList", data).then(res => {
        if (res.code == "SUCC") {
          var res = res.result;
          this.total = res.total;
          this.dataList = res.data;
        } else {
          this.$Message.warning(res.message);
        }
      });
    },
    open(id) {
        this.$prompt('请输入修改评论内容', '提示', {
            confirmButtonText: '确定',
            cancelButtonText: '取消',
        }).then(({ value }) => {
            this.updateContent = value
            this.handleUpdata(id)
        }).catch(() => {
            this.$message({
                type: 'info',
                message: '取消输入'
            })
        })
    },
    handleUpdata (id) {
        var data = {
            id: id,
            content : this.updateContent
        }
       this.$post("/admin/article_comment/update", data).then(res => {
        // this.$post("/admin/article_category/update", data).then(res => {
           console.log(res)
           if (res.code == "SUCC") {
               this.getOrderData()
               this.$Message.success(res.message)
           } else {
               this.$Message.warning(res.message)
           }
	    })
	    .catch(req => {
	    })
    }
  }
};
</script>

<style scoped>
.query-c {
  margin-bottom: 15px;
}
.ivu-page {
  text-align: right;
  margin-right: 30px;
}
</style>
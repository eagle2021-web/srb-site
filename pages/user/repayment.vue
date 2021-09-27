<template>
  <div class="personal-main">
    <div class="personal-money">
      <h3><i>资金记录</i></h3>

      <div class="personal-moneylist" style="margin-top: 40px;">
        <div class="pmain-contitle">
          <span class="pmain-title1 fb" style="width: 150px;">
            需还本金
          </span>
          <span class="pmain-title2 fb" style="width: 70px;">
            需还利息
          </span>
          <span class="pmain-title3 fb" style="width: 100px;">
            总共需还
          </span>
          <span class="pmain-title4 fb" style="width: 100px;">
            还款日期
          </span>
          <span class="pmain-title5 fb" style="width: 300px;">是否逾期</span>
        </div>
        <ul>
          <li
            v-for="repayment in repaymentList"
            :key="repayment.id"
            style="width:100%"
          >
            <span class="pmain-title1 pmain-height" style="width: 150px;">
              {{ repayment.principal }}
            </span>
            <span class="pmain-title2 pmain-height" style="width: 70px;">
              {{ repayment.interest }}
            </span>
            <span class="pmain-title3 pmain-height" style="width: 100px;">
              {{ repayment.total }}
            </span>
            <span class="pmain-title4 pmain-height" style="width: 100px;">
              {{ repayment.returnDate }}
            </span>
            <span
              :title="repayment.overdue"
              class="pmain-title5 pmain-height"
              style="width: 400px; overflow: hidden;"
            >
              {{ repayment.overdue }}
            </span>
          </li>
        </ul>
      </div>
      <el-pagination
          :current-page="page"
          :total="total"
          :page-size="limit"
          :page-sizes="[3, 5, 7]"
          style="padding: 30px 0; float: left"
          layout="total, sizes, prev, pager, next, ->, jumper"
          @size-change="changePageSize"
          @current-change="changeCurrentPage"
      />
    </div>
  </div>
</template>
<!--借款人还款计划-->
<script>
export default {
  data() {
    return {
      repaymentList: [],
      total: 0,
      page: 1,
      limit: 10
    }
  },

  mounted() {
    this.fetchRepaymentList()
  },

  methods: {
    changePageSize(size) {
      this.limit = size
      this.fetchRepaymentList()
    },
    changeCurrentPage(page) {
      this.page = page
      this.fetchRepaymentList()
    },
    fetchRepaymentList() {

      this.$axios.$get(`/api/core/lend/repayment/list/${this.page}/${this.limit}`).then((response) => {
        console.log("还款计划 = ", response)
        const pageModel = response.data.pageModel;
        this.repaymentList = pageModel.records || [];
        this.total = pageModel.total;
      })
    },
  },
}
</script>

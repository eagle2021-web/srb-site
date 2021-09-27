<template>
  <div class="personal-main" >
    <div class="personal-money" >
      <h3><i>借款记录</i></h3>
      <div class="personal-moneylist" style="margin-top: 40px;">
        <div class="pmain-contitle">
          <span class="pmain-title1 fb" style="width: 150px;">
            放款时间
          </span>
          <span class="pmain-title2 fb" style="width: 70px;">
            申请金额
          </span>
          <span class="pmain-title3 fb" style="width: 100px;">
            实际投资
          </span>
          <span class="pmain-title4 fb" style="width: 100px;">
            分期期数
          </span>
          <span class="pmain-title5 fb" style="width: 400px;">
            备注
          </span>
        </div>
        <ul>
          <li
            v-for="lend in borrow_list"
            :key="lend.id"
            style="width:100%"
          >
            <span class="pmain-title1 pmain-height" style="width: 150px;">
              {{ lend.paymentTime }}
            </span>
            <span class="pmain-title2 pmain-height" style="width: 70px;">
              {{ lend.amount }}
            </span>
            <span class="pmain-title3 pmain-height" style="width: 100px;">
              {{ lend.investAmount }}
            </span>
            <span class="pmain-title4 pmain-height" style="width: 100px;">
              {{ lend.period }}
            </span>
            <span
              :title="lend.title"
              class="pmain-title5 pmain-height"
              style="width: 400px; overflow: hidden;"
            >
              {{ lend.title }}
            </span>
          </li>
        </ul>
      </div>
    </div>

    <el-pagination
        :current-page="page"
        :total="total"
        :page-size="limit"
        :page-sizes="[1, 10, 30]"
        style="padding: 30px 0; float: left"
        layout="total, sizes, prev, pager, next, ->, jumper"
        @size-change="changePageSize"
        @current-change="changeCurrentPage"
    />
  </div>
</template>

<script>
export default {
  data() {
    return {
      borrow_list: [],
      page: 1,
      limit: 10,
      total: 0,
    }
  },

  mounted() {

    // this.fetchTransFlowList()
    this.fetchTransFlowList2()
    setInterval(() => {
      console.log(this.page)
    }, 1000)
  },

  methods: {
    changePageSize(size) {
      console.log('size', size)
      this.limit = size
      this.fetchTransFlowList2()
    },
    changeCurrentPage(page) {
      console.log('page', page)
      this.page = page
      this.fetchTransFlowList2()
    },
    // fetchTransFlowList() {
    //   this.$axios.$get('/api/core/lend/borrow/list').then((response) => {
    //     console.log("borrow-list", response)
    //     this.borrow_list = response.data.lends || []
    //   })
    // },
    fetchTransFlowList2() {
      this.$axios.$get('/api/core/lend/borrow/list/1/5').then((response) => {
        // console.log("borrow-list2", response)
        const model = response.data.pageModel;
        this.total = model.total;
        this.borrow_list = model.records || []
      })
    },
  },
}
</script>

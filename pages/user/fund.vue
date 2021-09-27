<template>
  <div class="personal-main">
    <div class="personal-money">
      <h3><i>资金记录</i></h3>
      <div class="personal-moneylist" style="margin-top: 40px;">
        <div class="pmain-contitle">
          <span class="pmain-title1 fb" style="width: 150px;">
            交易时间
          </span>
          <span class="pmain-title2 fb" style="width: 70px;">
            交易类型
          </span>
          <span class="pmain-title3 fb" style="width: 100px;">
            交易金额
          </span>
          <span class="pmain-title1 fb" style="width: 500px;text-align: center">备注</span>
        </div>
        <ul>
          <li
            v-for="transFlow in transFlowList"
            :key="transFlow.id"
            style="width:100%"
          >
            <span class="pmain-title1 pmain-height" style="width: 150px;">
              {{ transFlow.createTime }}
            </span>
            <span class="pmain-title2 pmain-height" style="width: 70px;">
              {{ transFlow.transTypeName }}
            </span>
            <span class="pmain-title3 pmain-height" style="width: 100px;">
              {{ transFlow.transAmount }}
            </span>
            <span
              :title="transFlow.memo"
              class="pmain-title1 pmain-height"
              style="width: 500px;display:block;text-overflow:ellipsis;white-space:nowrap;overflow:hidden;"
            >
              {{ transFlow.memo }}
            </span>
          </li>
        </ul>
      </div>
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
</template>

<script>
export default {
  data() {
    return {
      transFlowList: [],
      page: 1,
      limit: 5,
      total: 0
    }
  },

  mounted() {
    this.fetchTransFlowList()
  },

  methods: {
    changePageSize(size) {
      console.log('size', size)
      this.limit = size
      this.fetchTransFlowList()
    },
    changeCurrentPage(page) {
      console.log('page', page)
      this.page = page
      this.fetchTransFlowList()
    },
    fetchTransFlowList() {
      this.$axios.$get(`/api/core/transFlow/fund/list/${this.page}/${this.limit}`).then((response) => {
        console.log(response)
        const model = response.data.pageModel;
        this.total = model.total;
        this.transFlowList = model.records || []
      })
    },
  },
}
</script>

<template>
  <div class="personal-main">
    <div class="personal-money">
      <h3><i>投资记录</i></h3>

      <div class="personal-moneylist" style="margin-top: 40px;">
        <div class="pmain-contitle">
          <span class="pmain-title1 fb" style="width: 150px;">
            投资金额
          </span>
          <span class="pmain-title2 fb" style="width: 70px;">
            生效日期
          </span>
          <span class="pmain-title3 fb" style="width: 100px;">
            预期收益
          </span>
          <span class="pmain-title4 fb" style="width: 100px;">
            已获收益
          </span>
          <span class="pmain-title5 fb" style="width: 200px;">年利率</span>
        </div>
        <ul>
          <li
            v-for="invest in investList"
            :key="invest.id"
            style="width:100%"
          >
            <span class="pmain-title1 pmain-height" style="width: 150px;">
              {{ invest.investAmount }}
            </span>
            <span class="pmain-title2 pmain-height" style="width: 70px;">
              {{ invest.lendStartDate }}
            </span>
            <span class="pmain-title3 pmain-height" style="width: 100px;">
              {{ invest.expectAmount }}
            </span>
            <span class="pmain-title4 pmain-height" style="width: 100px;">
              {{ invest.realAmount }}
            </span>
            <span
              :title="invest.lendYearRate"
              class="pmain-title5 pmain-height"
              style="width: 200px; overflow: hidden;"
            >
              {{ invest.lendYearRate }}
            </span>
          </li>
        </ul>
      </div>
      <el-pagination
          :current-page="page"
          :total="total"
          :page-size="limit"
          :page-sizes="[3, 5, 10]"
          style="padding: 30px 0; float: left"
          layout="total, sizes, prev, pager, next, ->, jumper"
          @size-change="changePageSize"
          @current-change="changeCurrentPage"
      />
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      investList: [],
      page: 1,
      limit: 10,
      total: 0,
    }
  },


  mounted() {
    this.fetchInvestList()
  },

  methods: {
    changePageSize(size) {
      this.limit = size
      this.fetchInvestList()
    },
    changeCurrentPage(page) {
      this.page = page
      this.fetchInvestList()
    },
    // createTime: "2021-09-22 19:41:23"
    // deleted: false
    // expectAmount: 2400
    // id: 5
    // investAmount: 20000
    // investName: "18128322006"
    // investTime: "2021-09-22 19:41:23"
    // investUserId: 24
    // lendEndDate: "2022-09-23"
    // lendId: 4
    // lendItemNo: "INVEST2021092219412379f1395f-a"
    // lendStartDate: "2021-09-23"
    // lendYearRate: 0.12
    // realAmount: 400
    // status: 1
    // updateTime: "2021-09-22 19:41:23"
    fetchInvestList() {
      this.$axios.$get(`/api/core/lendItem/invest/list/${this.page}/${this.limit}`).then((response) => {
        console.log("投资列表 = ", response.data.pageModel);
        const pageModel = response.data.pageModel;
        this.investList = pageModel.records;
        this.total = pageModel.total;
      })
    },
  },
}
</script>

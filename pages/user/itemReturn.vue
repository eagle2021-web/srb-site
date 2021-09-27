<template>
  <div class="personal-main">
    <div class="personal-money">
      <h3><i>回款记录</i></h3>

      <div class="personal-moneylist" style="margin-top: 40px;">
        <div class="pmain-contitle">
          <span class="pmain-title1 fb" style="width: 150px;">
            本期所得利息
          </span>
          <span class="pmain-title2 fb" style="width: 70px;">
            回款总额
          </span>
          <span class="pmain-title3 fb" style="width: 100px;">
            预定回款日期
          </span>
          <span class="pmain-title4 fb" style="width: 100px;">
            实际回款日期
          </span>
<!--          <span class="pmain-title5 fb" style="width: 200px;">年利率</span>-->
        </div>
        <ul>
          <li
            v-for="invest in investList"
            :key="invest.id"
            style="width:100%"
          >
            <span class="pmain-title1 pmain-height" style="width: 150px;">
              {{ invest.interest }}
            </span>
            <span class="pmain-title2 pmain-height" style="width: 70px;">
              {{ invest.total }}
            </span>
            <span class="pmain-title3 pmain-height" style="width: 100px;">
              {{ invest.returnDate }}
            </span>
            <span class="pmain-title4 pmain-height" style="width: 100px;">
              {{ invest.realRetrunDate }}
            </span>
<!--            <span-->
<!--              :title="invest.lendYearRate"-->
<!--              class="pmain-title5 pmain-height"-->
<!--              style="width: 200px; overflow: hidden;"-->
<!--            >-->
<!--              {{ invest.lendYearRate }}-->
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
    fetchInvestList() {
      this.$axios.$get(`/api/core/lendItemReturn/investor/list/${this.page}/${this.limit}`).then((response) => {
        console.log("回款列表 = ", response.data.pageModel);
        const pageModel = response.data.pageModel;
        this.investList = pageModel.records;
        this.total = pageModel.total;
      })
    },
  },
}
</script>

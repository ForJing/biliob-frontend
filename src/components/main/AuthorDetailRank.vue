<template>
  <VCard>
    <VCardText style="height:100%">
      <div class="rank-container">
        <div style="flex-grow: 1;text-align:center">
          <div class="rank-title subheading font-weight-bold grey--text text--darken-2">
            粉丝数排名
          </div>
          <div class="title font-weight-black blue--text text--darken-3">
            {{fansInfo}}
          </div>
          <ChevronBudget :value="dFansRank"></ChevronBudget>
        </div>
        <div>
          <VDivider vertical></VDivider>
        </div>
        <div style="flex-grow: 1;text-align:center">
          <div class="rank-title subheading font-weight-bold grey--text text--darken-2">
            播放量排名
          </div>
          <div class="title font-weight-black blue--text text--darken-3">
            {{archiveInfo}}
          </div>
          <ChevronBudget :value="dArchiveViewRank"></ChevronBudget>
        </div>
        <div>
          <VDivider vertical></VDivider>
        </div>
        <div style="flex-grow: 1;text-align:center">
          <div class="rank-title subheading font-weight-bold grey--text text--darken-2">
            专栏阅读排名
          </div>
          <div class="title font-weight-black blue--text text--darken-3">
            {{articleInfo}}
          </div>
          <ChevronBudget :value="dArticleViewRank"></ChevronBudget>
        </div>
      </div>
    </VCardText>
    <VCardText
      class="caption grey--text"
      style="display:flex;justify-content:space-between"
    >
      <div>
        仅包括已经观测UP主
      </div>
      <div>
        {{formatedDate}}
      </div>
    </VCardText>
  </VCard>
</template>

<script>
var format = require("date-fns/format");
var { convertDateToUTC } = require("../../charts/util/convertDateToUTC");
import ChevronBudget from "../common/ChevronBudget.vue";
export default {
  components: { ChevronBudget },
  props: {
    fansRank: Number(),
    articleViewRank: Number(),
    archiveViewRank: Number(),
    dFansRank: Number(),
    dArticleViewRank: Number(),
    dArchiveViewRank: Number(),
    pFansRank: Number(),
    pArticleViewRank: Number(),
    pArchiveViewRank: Number(),
    updateTime: String()
  },
  computed: {
    formatedDate() {
      if (this.updateTime != undefined) {
        return format(
          convertDateToUTC(new Date(this.updateTime.replace("+0000", ""))),
          "YYYY-MM-DD HH:MM:SS"
        );
      } else {
        return "载入更新时间中";
      }
    },
    fansInfo() {
      if (this.fansRank == undefined) {
        return "";
      }
      if (this.fansRank <= 200 && this.fansRank != -1) {
        return `Top ${this.fansRank}`;
      } else {
        return this.beautify(this.pFansRank);
      }
    },
    articleInfo() {
      if (this.articleViewRank == undefined) {
        return "";
      }
      if (this.articleViewRank <= 200 && this.articleViewRank != -1) {
        return `Top ${this.articleViewRank}`;
      } else {
        return this.beautify(this.pArticleViewRank);
      }
    },
    archiveInfo() {
      if (this.archiveViewRank == undefined) {
        return "";
      }
      if (this.archiveViewRank <= 200 && this.archiveViewRank != -1) {
        return `Top ${this.archiveViewRank}`;
      } else {
        return this.beautify(this.pArchiveViewRank);
      }
    }
  },

  methods: {
    beautify(val) {
      if (val === -1) {
        return "-";
      } else {
        return `前${val}%`;
      }
    }
  }
};
</script>

<style scoped>
.rank-container {
  display: flex;
  align-items: stretch;
  justify-content: center;
  height: 100%;
}
</style>

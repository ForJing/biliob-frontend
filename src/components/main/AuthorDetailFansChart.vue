<template>
  <VCard class="card">
    <VResponsive :aspect-ratio="8/5">
      <Chart
        class="chart"
        :theme="theme"
        :auto-resize="true"
        :options="fans"
      />
    </VResponsive>
  </VCard>
</template>

<script>
import drawFansChart from "../../charts/author-fans.js";
var deepCopy = function(o) {
  if (o instanceof Array) {
    var n = [];
    for (let i = 0; i < o.length; ++i) {
      n[i] = deepCopy(o[i]);
    }
    return n;
  } else if (o instanceof Object) {
    var no = {};
    for (let i in o) {
      no[i] = deepCopy(o[i]);
    }
    return no;
  } else {
    return o;
  }
};
export default {
  props: { authorData: Object() },
  data() {
    return {
      fans: Object(),
      theme: "colorful"
    };
  },
  computed: {
    isDark() {
      return this.$store.state.dark;
    }
  },
  watch: {
    authorData: function(val) {
      this.fans = drawFansChart(deepCopy(val));
    },
    isDark(val) {
      if (val) {
        this.theme = "mydark";
      } else {
        this.theme = "colorful";
      }
    }
  },
  mounted() {
    if (this.$store.state.dark) {
      this.theme = "mydark";
    } else {
      this.theme = "colorful";
    }
  }
};
</script>

<style scoped>
.video-img {
  height: 70px;
  border-radius: 4px;
}
.chart {
  width: 100%;
  height: 100%;
}
</style>

<template>
    <div ref="map" class="map"></div>
</template>

<script>
import Echarts from "echarts";
export default {
  data() {
    return {
      chart: null
    };
  },
  created() {
    this.chart = Echarts.init(this.$refs.map);
    //this.chart.register()
  },
  methods: {
    onChartClicke(e) {
      console.log(e);
    }
  }
};
</script>

<style>
.map {
  width: 600px;
  height: 600px;
}
</style>

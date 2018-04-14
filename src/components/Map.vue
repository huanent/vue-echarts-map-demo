<template>
  <div ref="map" class="map"></div>
</template>

<script>
import Echarts from "echarts";
import Axios from "axios";
import CityMap from "../../map/city-map";
import ProvinceMap from "../../map/province-map";
import provinceMap from '../../map/province-map';

export default {
  data() {
    return {
      chart: null,
      option: {
        title: {
          text: "全国地图"
        },
        series: [
          {
            type: "map",
            mapType: "china",
            selectedMode: "single"
          }
        ]
      }
    };
  },
  async mounted() {
    this.chart = Echarts.init(this.$refs.map);
    this.chart.showLoading();
    let mapJson = await Axios.get(
      "https://raw.githubusercontent.com/huanent/vue-echarts-map-demo/master/map/china.json"
    );
    Echarts.registerMap("china", mapJson.data);
    this.chart.setOption(this.option);
    this.chart.on("mapselectchanged", this.onProvinceClick);
    this.chart.hideLoading();
  },
  methods: {
    async onProvinceClick(e) {
      this.chart.showLoading();
      let name = e.batch[0].name;
      let pinyinName = provinceMap[name]
      let mapJson = await Axios.get(
        `https://raw.githubusercontent.com/huanent/vue-echarts-map-demo/master/map/province/${pinyinName}.json`
      );
      Echarts.registerMap(pinyinName, mapJson.data);
      this.option.series[0].mapType = pinyinName;
      this.chart.setOption(this.option);
      this.chart.off("mapselectchanged", this.onProvinceClick);
      this.chart.on("mapselectchanged", this.onCityClick);
      this.chart.hideLoading();
    },
    async onCityClick(e) {
      this.chart.showLoading();
      let name = e.batch[0].name;
      var cityCode = CityMap[name];
      let mapJson = await Axios.get(
        `https://raw.githubusercontent.com/huanent/vue-echarts-map-demo/master/map/citys/${cityCode}.json`
      );
      Echarts.registerMap(cityCode, mapJson.data);
      this.option.series[0].mapType = cityCode;
      this.chart.setOption(this.option);
      this.chart.off("mapselectchanged", this.onCityClick);
      this.chart.hideLoading();
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

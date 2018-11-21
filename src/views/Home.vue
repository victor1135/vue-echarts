<template>
  <div class="home">
    <MyHead></MyHead>
    <MyHead></MyHead>
    <MyHead></MyHead>
    <button @click="initChart"></button>
    <div id="box" style="width:960px;height:500px;margin:0 auto;"></div>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";
import HelloWorld from "@/components/HelloWorld.vue"; // @ is an alias to /src
import MyHead from "@/components/MyHead.vue"; // @ is an alias to /src
import * as echarts from "echarts/lib/echarts";
import "echarts/lib/chart/line";
import "echarts/lib/component/tooltip";
import "echarts/lib/component/title";
import "echarts/lib/component/dataZoom";
const jsonData = require("../assets/data.json");
@Component({
  components: {
    HelloWorld,
    MyHead
  }
})
export default class Home extends Vue {
  initChart() {
    const data = jsonData;
    console.log(data);
    console.log(echarts);
    const myChart = echarts.init(document.getElementById("box"));
    const option = {
      dataZoom: [
        {
          type: "slider",
          show: true,
          start: 94,
          end: 100,
          handleSize: 5
        }
      ],
      grid: { containLabel: true },
      title: {
        text: "阿發盃"
      },
      tooltip: {
        trigger: "axis"
      },
      xAxis: {
        data: data.map(item => {
          return item[0];
        })
      },
      yAxis: {
        splitLine: {
          show: false
        }
      },
      toolbox: {
        left: "center",
        feature: {
          dataZoom: {
            yAxisIndex: "none"
          },
          restore: {},
          saveAsImage: {}
        }
      },
      series: {
        symbolSize: 20,
        name: "Beijing AQI",
        type: "line",
        data: data.map(item => {
          return item[1];
        }),
        lineStyle: {
        width: 0,
        color: 'rgba(0, 0, 0, 0)'
        },
      }
    };
    myChart.setOption(option);
    myChart.on("datazoom", function(evt) {
      var axis = myChart.getModel().option.xAxis[0];
      var starttime = axis.data[axis.rangeStart];
      var endtime = axis.data[axis.rangeEnd];
      console.log(starttime, endtime);
    });
  }
}
</script>

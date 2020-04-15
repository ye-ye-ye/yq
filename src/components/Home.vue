<template>
  <div class="hello">
    <div id="echart"></div>
  </div>
</template>

<script>
import echarts from "echarts";
import jsonp from "jsonp";
import "echarts/map/js/china";

var option = {
  title: {
    text: " vue 实现疫情地图", //地图标题。
    x: "center" //标题水平居中。
  },
  //工具栏。内置有导出图片，数据视图，动态类型切换，数据区域缩放，重置五个工具。
  toolbox: {
    show: true,
    feature: {
      // dataZoom: {
      //   yAxisIndex: "none"
      // },
      dataView: { readOnly: false },
      restore: {},
      saveAsImage: {}
    }
  },
  series: [
    //地图数据
    {
      name: "疫情地图",
      type: "map",
      mapType: "china", // 自定义扩展图表类型
      label: {
        //地图文本标签。可用于说明图形的一些数据信息。
        show: true,
        color: "black",
        fontStyle: "oblique"
      },
      zoom: 1, //地图比例
      itemStyle: { borderColor: "blue" }, //地图区域的多边形 图形样式。
      data: [],
      emphasis: {
        //高亮状态下的设置。
        label: {
          //图形上的文本标签，可用来说明一些图形数据信息。
          color: "#fff",
          fontSize: 12
        },
        itemStyle: { areaColor: "green" } //地图区域的多边形图形样式。
      }
    }
  ],
  tooltip: {
    trigger: "item",
    formatter: "地区：{b}<br/>  确诊 ：{c} "
  },
  visualMap: {
    type: "piecewise",
    show: true,
    pieces: [
      { min: 10000 }, // 不指定 max，表示 max 为无限大（Infinity）。
      { min: 2000, max: 9999 },
      { min: 500, max: 1999 },
      { min: 200, max: 499 },
      { max: 199 }
    ],

    inRange: {
      color: ["lightskyblue", "yellow", "orangered"]
    }
  }
};

export default {
  name: "HelloWorld",
  data() {
    return { myChart: "" };
  },
  mounted() {
    this.getData();
    //基于准备好的dom ，初始化echarts实例。
    this.myChart = echarts.init(document.getElementById("echart"));
    this.myChart.setOption(option);
  },

  methods: {
    getData() {
      var that = this;
      jsonp(
        "https://interface.sina.cn/news/wap/fymap2020_data.d.json?_=1580892522427",
        function(err, data) {
          var arr = data.data.list.map(item => {
            return { name: item.name, value: item.value };
          });
          option.series[0].data = arr;
          //使用刚指定的配置和数据显示图表。
          that.myChart.setOption(option);
          console.log(data);
        }
      );
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1,
h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
#echart {
  width: 800px;
  height: 500px;
  background-color: bisque;
  margin: 0px auto;
}
</style>

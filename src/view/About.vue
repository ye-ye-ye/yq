<template>
  <div id="about" style="height:500px; width:800px;margin:0px auto; "></div>
</template>
<script>
import echarts from "echarts";
import jsonp from "jsonp";
var option = {
  title: {
    text: "全国现有确诊/现有疑似"
  },
  tooltip: {
    trigger: "axis"
  },
  legend: {
    data: ["现有确诊", "现有疑似"]
  },
  //工具栏。内置有导出图片，数据视图，动态类型切换，数据区域缩放，重置五个工具。
  toolbox: {
    show: true,
    feature: {
      dataZoom: {
        yAxisIndex: "none"
      },
      dataView: { readOnly: false },
      magicType: { type: ["line", "bar"] },
      restore: {},
      saveAsImage: {}
    }
  },
  xAxis: {
    type: "category",

    boundaryGap: false,
    data: [1, 2, 3]
  },
  yAxis: {
    type: "value",
    axisLabel: {
      formatter: "{value} "
    }
  },
  series: [
    {
      name: "现有确诊",
      type: "line",
      data: [],
      markPoint: {
        data: [
          { type: "max", name: "最大值" },
          { type: "min", name: "最小值" }
        ]
      },
      markLine: {
        data: [{ type: "average", name: "平均值" }]
      }
    },
    {
      name: "现有疑似",
      type: "line",
      data: [],
      markPoint: {
        // data: [{ name: "周最低", value: -2, xAxis: 1, yAxis: -1.5 }]
      },
      markLine: {
        data: [
          { type: "average", name: "平均值" },
          [
            {
              symbol: "none",
              x: "90%",
              yAxis: "max"
            },
            {
              symbol: "circle",
              label: {
                position: "start",
                formatter: "最大值"
              },
              type: "max",
              name: "最高点"
            }
          ]
        ]
      }
    }
  ]
};

export default {
  name: "about",
  data() {
    return {
      myAchart: ""
    };
  },
  mounted() {
    //基于准备好的dom ，初始化echarts实例
    this.getData();
    this.myAchart = echarts.init(document.getElementById("about"));
    //使用指定的配置和数据来展现图表。
    this.myAchart.setOption(option);
  },
  methods: {
    getData() {
      jsonp(
        "https://interface.sina.cn/news/wap/fymap2020_data.d.json?_=1580892522427",
        function(err, data) {
          //y轴坐标
          option.xAxis.data = data.data.historylist
            .map(item => item.date)
            .reverse();
          //现有确诊
          option.series[0].data = data.data.historylist
            .map(item => item.cn_econNum)
            .reverse();
          //现有疑似
          option.series[1].data = data.data.historylist
            .map(item => item.cn_susNum)
            .reverse();
          console.log(data.data.historylist);

          this.myAchart.setOption(option);
        }
      );
    }
  }
};
</script>
<style scoped>
</style>
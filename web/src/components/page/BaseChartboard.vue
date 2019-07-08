<template>
    <div>
        <div class="crumbs">
            <el-breadcrumb separator="/">
                <el-breadcrumb-item><i class="el-icon-date"></i> 图表</el-breadcrumb-item>
                <el-breadcrumb-item>统计信息</el-breadcrumb-item>
            </el-breadcrumb>
        </div>
        <div class="container">
             <div class="vchart-box">
                <v-chart class="vchart" :options="bar"/>
             </div>
             <div class="vchart-box">
                <v-chart class="vchart" :options="polar"/>
             </div>
             <div class="vchart-box">
             <v-chart class="vchart" :options="gauge" />
             </div>
             <div class="vchart-box">
             <v-chart class="vchart" :options="pie" />
             </div>
             <div class="vchart-box">
             <v-chart :options="chinachart"/>
             </div>
        </div>
    </div>
</template>

<script>
import ECharts from "vue-echarts/components/ECharts";
import "echarts/lib/chart/bar";
import "echarts/lib/chart/line";
import "echarts/lib/chart/gauge";
import "echarts/lib/chart/pie";
import "echarts/lib/component/polar";
import "echarts/map/js/china.js";
export default {
  name: "basecharts",
  components: {
    "v-chart": ECharts
  },
  data: function() {
    let data = [];
    for (let i = 0; i <= 360; i++) {
      let t = i / 180 * Math.PI;
      let r = Math.sin(2 * t) * Math.cos(2 * t);
      data.push([r, i]);
    }
    return {
      chinachart: {
        tooltip: {}, // 鼠标移到图里面的浮动提示框
        visualMap: {
          //左侧小柱子的配置
          min: 0, // 最小值
          max: 15000, //最大值
          left: "left", // 定位左边
          top: "bottom", // 定位底部
          text: ["高", "低"], // 上下两个文字
          seriesIndex: [1],
          inRange: {
            color: ["#e0ffff", "#006edd"] // 深浅颜色
          },
          calculable: true // 显示与否
        },
        geo: {
          // 这个是重点配置区
          map: "china", // 表示中国地图
          roam: true,
          label: {
            normal: {
              show: true, // 是否显示对应地名
              textStyle: {
                color: "rgba(0,0,0,0.4)"
              }
            }
          },
          itemStyle: {
            normal: {
              borderColor: "rgba(0, 0, 0, 0.2)"
            },
            emphasis: {
              areaColor: null,
              shadowOffsetX: 0,
              shadowOffsetY: 0,
              shadowBlur: 20,
              borderWidth: 0,
              shadowColor: "rgba(0, 0, 0, 0.5)"
            }
          }
        },
        series: [
          {
            type: "scatter",
            coordinateSystem: "geo" // 对应上方配置
          },
          {
            name: "启动次数", // 浮动框的标题
            type: "map",
            geoIndex: 0,
            data: [{ name: "广东", value: 1324 }] // 这里就是数据，即数组可以单独放在外面也可以直接写
          }
        ]
      },
      pie: {
        backgroundColor: "#ffffff", //"#2c343c",

        title: {
          text: "Customized Pie",
          left: "center",
          top: 20,
          textStyle: {
            color: "#ffffff"
          }
        },

        tooltip: {
          trigger: "item",
          formatter: "{a} <br/>{b} : {c} ({d}%)"
        },

        visualMap: {
          show: false,
          min: 80,
          max: 600,
          inRange: {
            colorLightness: [0, 1]
          }
        },
        series: [
          {
            name: "访问来源",
            type: "pie",
            radius: "55%",
            center: ["50%", "50%"],
            data: [
              { value: 335, name: "直接访问" },
              { value: 310, name: "邮件营销" },
              { value: 274, name: "联盟广告" },
              { value: 235, name: "视频广告" },
              { value: 400, name: "搜索引擎" }
            ].sort(function(a, b) {
              return a.value - b.value;
            }),
            roseType: "radius",
            label: {
              normal: {
                textStyle: {
                  color: "rgba(5, 5, 5, 0.3)"
                }
              }
            },
            labelLine: {
              normal: {
                lineStyle: {
                  color: "rgba(5, 5, 5, 0.3)"
                },
                smooth: 0.2,
                length: 10,
                length2: 20
              }
            },
            itemStyle: {
              normal: {
                color: "#c23531",
                shadowBlur: 200,
                shadowColor: "rgba(0, 0, 0, 0.5)"
              }
            },

            animationType: "scale",
            animationEasing: "elasticOut",
            animationDelay: function(idx) {
              return Math.random() * 200;
            }
          }
        ]
      },
      gauge: {
        tooltip: {
          formatter: "{a} <br/>{b} : {c}%"
        },
        toolbox: {
          feature: {
            restore: {},
            saveAsImage: {}
          }
        },
        series: [
          {
            name: "业务指标",
            type: "gauge",
            detail: { formatter: "{value}%" },
            data: [{ value: 50, name: "完成率" }]
          }
        ]
      },
      bar: {
        title: {
          text: "2019年使用柱状图"
        },
        tooltip: {},
        legend: {
          data: ["销量"]
        },
        xAxis: {
          data: ["衬衫", "羊毛衫", "雪纺衫", "裤子", "高跟鞋", "袜子"]
        },
        yAxis: {},
        series: [
          {
            name: "销量",
            type: "bar",
            data: [5, 20, 36, 10, 10, 20]
          }
        ]
      },
      polar: {
        title: {
          text: "极坐标双数值轴"
        },
        legend: {
          data: ["line"]
        },
        polar: {
          center: ["50%", "54%"]
        },
        tooltip: {
          trigger: "axis",
          axisPointer: {
            type: "cross"
          }
        },
        angleAxis: {
          type: "value",
          startAngle: 0
        },
        radiusAxis: {
          min: 0
        },
        series: [
          {
            coordinateSystem: "polar",
            name: "line",
            type: "line",
            showSymbol: false,
            data: data
          }
        ],
        animationDuration: 2000
      }
    };
  }
};
</script>

<style scoped>
.vchart-box {
  display: inline-block;
  margin: 20px;
}
.vchart {
  width: 500px;
  height: 400px;
}
.content-title {
  clear: both;
  font-weight: 400;
  line-height: 50px;
  margin: 10px 0;
  font-size: 22px;
  color: #1f2f3d;
}
</style>
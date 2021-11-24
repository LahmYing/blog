---
title: echarts
date: 2021-03-16 16:30:37
tags: echarts
category: []
---

<!-- toc -->

# myEcharts.js

```js
import * as echarts from "echarts";

const install = function (Vue) {
  Object.defineProperties(Vue.prototype, {
    $chart: {
      get() {
        return {
          // 柱状图
          bar: function (id, title, xAxis, yAxis, dataList) {
            this.chart = echarts.init(document.getElementById(id));
            this.chart.clear();

            const optionData = {
              // 直角坐标系内绘图网格，单个 grid 内最多可以放置上下两个 X 轴，左右两个 Y 轴。可以在网格上绘制折线图，柱状图，散点图（气泡图）。
              // 在 ECharts 2.x 里单个 echarts 实例中最多只能存在一个 grid 组件，在 ECharts 3 中可以存在任意个 grid 组件
              grid: {
                // width: 280,
                // left: 'center',
              },
              title: {
                text: title,
                left: "center",
                textStyle: {
                  fontSize: "17px",
                  fontFamily: "Microsoft YaHei",
                  fontWeight: 400,
                  color: "#333333",
                },
              },
              tooltip: {
                formatter(params) {
                  return params.name + "<br/>值：" + params.value;
                },
                triggerOn: "mousemove",
                extraCssText: "text-align: left;",
              },
              // 缩放、滑动
              dataZoom: [
                {
                  type: "slider",
                  start: 0,
                  end: 50,
                  show: true,
                },
                {
                  start: 0,
                  end: 50,
                },
              ],
              xAxis: xAxis,
              yAxis: yAxis,
              series: [
                {
                  data: dataList,
                  type: "bar",
                  itemStyle: {
                    color: "#2E6CFB",
                    barBorderRadius: [5, 5, 0, 0],
                  },
                  // 柱标签
                  label: {
                    show: true,
                    position: "top",
                  },
                },
              ],
            };

            this.chart.setOption(optionData);
          },
          // 2折线图
          mutiLine: function (id, title, lineNames, xAxis, yAxis, series) {
            this.chart = echarts.init(document.getElementById(id));
            this.chart.clear();

            const optionData = {
              backgroundColor: "#fff",
              grid: {
                left: "25",
                right: "25",
                bottom: "24",
                top: "75",
                containLabel: true,
              },
              legend: {
                data: lineNames,
                orient: "horizontal",
                icon: "rect",
                show: true,
                left: 20,
                top: 25,
              },
              title: {
                text: title || "",
                left: "18px",
                top: "0",
                textStyle: {
                  color: "#999",
                  fontSize: 12,
                  fontWeight: "400",
                },
              },
              color: ["#73A0FA", "#FFB761"],
              tooltip: {
                trigger: "axis",
                axisPointer: {
                  type: "cross",
                  crossStyle: {
                    color: "#999",
                  },
                  lineStyle: {
                    type: "dashed",
                  },
                },
              },
              dataZoom: {
                start: 0,
                type: "inside",
              },
              xAxis: xAxis,
              yAxis: yAxis,
              series: series,
            };

            this.chart.setOption(optionData);
          },
        };
      },
    },
  });
};

export default {
  install,
};
```

# main.js

```js
import myCharts from "./common/myCharts";
Vue.use(myCharts);
```

# 组件中使用

```js
this.$chart.mutiLine("line1", "", ["扫码次数", "留资人数"], xAxis, yAxis, [
  {
    name: "扫码次数",
    type: "line",
    // stack: '总量',
    data: this.scanQrcodeData,
    smooth: false,
  },
  {
    name: "留资人数",
    type: "line",
    // 设置折线图不堆叠只需要将每一个stack的值设置为不一样的名称或者将stack属性删除即可
    // stack: '总量',
    data: this.leaveCapitaData,
    smooth: false,
  },
]);
```

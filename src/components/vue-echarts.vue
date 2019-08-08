<template>
  <!-- 行情图 -->
  <div class="charts">
    <div class="minute" id="kcharts"></div>
    <!-- k线分类 -->
    <van-grid class="selbar" :border="false" :column-num="8">
      <van-grid-item v-for="(kty,kdex) in selbar" :key="kdex" @click="changeKty(kty,kdex)">
        <span :class="({'items':kdex!=currentkty,'itemsHL':kdex===currentkty})">{{kty.name}}</span>
      </van-grid-item>
    </van-grid>
  </div>
</template>

<script>
var echarts = require('echarts')
export default {
  name: 'Kcharts',
  props: {},
  data() {
    return {
      // k线分类
      selbar: [
        { name: '分时', val: 'quotek1' },
        { name: '1分', val: 'quotek1' },
        { name: '5分', val: 'quotek5' },
        { name: '15分', val: 'quotek15' },
        { name: '60分', val: 'quotek60' },
        { name: '日K', val: 'quotekd' },
        { name: '周K', val: 'quotekw' },
        { name: '月K', val: 'quotekm' }
      ],
      currentkty: 0
    }
  },
  methods: {
    // 切换k线类别
    changeKty(kty, kdex) {
      this.currentkty = kdex
      if (kdex > 0) {
        this.drawKline()
      } else {
        this.drawLine()
      }
    },
    // 绘制k线
    drawKline() {
      var myChart = echarts.init(document.getElementById('kcharts'))
      var option = {
        tooltip: {
          trigger: 'axis',
          axisPointer: {
            type: 'cross',
            label: {
              backgroundColor: '#283b56'
            }
          }
        },
        dataZoom: {
          show: false,
          start: 0,
          end: 100
        },
        xAxis: [
          {
            type: 'category',
            boundaryGap: true,
            data: (function() {
              var now = new Date()
              var res = []
              var len = 10
              while (len--) {
                res.unshift(now.toLocaleTimeString().replace(/^\D*/, ''))
                now = new Date(now - 2000)
              }
              return res
            })()
          },
          {
            type: 'category',
            boundaryGap: true,
            data: (function() {
              var res = []
              var len = 10
              while (len--) {
                res.push(10 - len - 1)
              }
              return res
            })()
          }
        ],
        yAxis: [
          {
            type: 'value',
            scale: true,
            name: '价格',
            max: 30,
            min: 0,
            boundaryGap: [0.2, 0.2]
          },
          {
            type: 'value',
            scale: true,
            name: '预购量',
            max: 1200,
            min: 0,
            boundaryGap: [0.2, 0.2]
          }
        ],
        series: [
          {
            name: '最新成交价',
            type: 'line',
            data: (function() {
              var res = []
              var len = 0
              while (len < 10) {
                res.push((Math.random() * 10 + 5).toFixed(1) - 0)
                len++
              }
              return res
            })(),
          }
        ]
      }

      app.count = 11
      setInterval(function() {
        var axisData = new Date().toLocaleTimeString().replace(/^\D*/, '')

        var data0 = option.series[0].data
        data0.shift()
        data0.push(Math.round(Math.random() * 30))

        option.xAxis[0].data.shift()
        option.xAxis[0].data.push(axisData)

        myChart.setOption(option)
      }, 2000)

      myChart.setOption(option, true)
    },
    // 绘制分时线
    drawLine() {
      var myChart = echarts.init(document.getElementById('kcharts'))
      var base = +new Date(1968, 9, 3)
      var oneDay = 24 * 3600 * 1000
      var date = []
      var data = [Math.random() * 300]
      for (var i = 1; i < 20000; i++) {
        var now = new Date((base += oneDay))
        date.push(
          [now.getFullYear(), now.getMonth() + 1, now.getDate()].join('/')
        )
        data.push(Math.round((Math.random() - 0.5) * 20 + data[i - 1]))
      }
      let option = {
        grid: {
          top: '2%',
          left: '2%',
          right: '14%',
          bottom: '8%'
        },
        tooltip: {
          triggerOn: 'click',
          trigger: 'axis',
          axisPointer: {
            type: 'cross',
            label: {
              fontSize: 12,
              padding: [2, 5],
              color: '#333333',
              height: '20px',
              backgroundColor: 'rgba(255,255,255,1)',
              borderRadius: 3,
              borderWidth: 1,
              borderColor: 'rgba(178,178,178,1)',
              shadowBlur: 0
            }
          },
          // 图例样式
          backgroundColor: 'rgba(255,255,255,1)',
          borderWidth: 1,
          borderColor: 'rgba(221,221,221,1)',
          padding: 10,
          textStyle: {
            color: '#333333',
            fontSize: 12
          }
        },
        xAxis: {
          type: 'category',
          position: 'bottom',
          data: date,
          axisLine: {
            lineStyle: {
              color: '#7D7D7D'
            }
          }
        },
        yAxis: {
          type: 'value',
          scale: true,
          position: 'right',
          // Y轴刻度
          axisLine: {
            lineStyle: {
              color: '#7D7D7D'
            }
          },
          // Y轴分割线
          splitLine: {
            lineStyle: {
              color: '#f8f8f8'
            }
          }
        },
        dataZoom: {
          show: true,
          type: 'inside',
          start: 60,
          end: 100
        },
        series: [
          {
            name: 'BTC',
            type: 'line',
            smooth: false,
            symbol: 'none',
            sampling: 'average',
            itemStyle: {
              color: '#108EE9'
            },
            lineStyle: {
              normal: {
                width: 1,
                color: 'rgba(16,142,233,0.6)'
              }
            },
            // 渐变色填充
            areaStyle: {
              color: new echarts.graphic.LinearGradient(0, 0, 0, 1, [
                {
                  offset: 0,
                  color: 'rgba(16, 142, 233, 0.58)'
                },
                {
                  offset: 1,
                  color: 'rgba(16, 142, 233, 0)'
                }
              ])
            },
            // 基准线
            markLine: {
              silent: true, //不响应点击事件
              symbol: 'none',
              data: [
                {
                  yAxis: data[data.length - 10]
                }
              ],
              // 标线文本样式
              label: {
                normal: {
                  textStyle: {
                    fontSize: 12,
                    padding: [2, 5],
                    color: '#108EE9',
                    height: '20px',
                    backgroundColor: 'rgba(247,252,255,1)',
                    borderRadius: 3,
                    borderWidth: '1px',
                    borderColor: 'rgba(16,142,233,0.5)'
                  }
                }
              }
            },
            data: data
          }
        ]
      }
      myChart.setOption(option, true)
    }
  },
  mounted() {
    this.drawKline()
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.charts {
  height: 380px;
  background: #ffffff;
}
.charts .minute {
  width: 348px;
  height: 336px;
  margin: 0 auto;
  background: rgba(255, 255, 255, 1);
  border-radius: 6px;
  border: 1px solid rgba(240, 240, 240, 1);
}
.charts .selbar {
  margin-top: 8px;
  height: 36px;
  background-color: #6cc47c;
  font-size: 12px;
}
.charts .selbar .items {
  color: #7d7d7d;
  border-bottom: 2px solid #ffffff;
  padding-bottom: 2px;
}
.charts .selbar .itemsHL {
  color: #108ee9;
  border-bottom: 2px solid #108ee9;
  padding-bottom: 2px;
}
</style>

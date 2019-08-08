<template>
  <div class="content">
    <div class="kline">
      <div class="title">
        <p>
          BTC
          <span>3975.86（+2.05%）</span>
        </p>
      </div>
      <div id="main"></div>
    </div>
    <div class="kbar">
      <div
        :class="{'item': acIndex != index, 'active': acIndex == index}"
        v-for="(item,index) in bar"
        :key="index"
        @click="changeBar(index)"
      >{{ item.name }}</div>
    </div>
  </div>
</template>

<script>
var echarts = require('echarts')
export default {
  data() {
    return {
      num: 0,
      bar: [
        { name: '分时线', val: 'quotek1' },
        { name: '1分线', val: 'quotek1' },
        { name: '5分线', val: 'quotek5' },
        { name: '15分线', val: 'quotek15' },
        { name: '60分线', val: 'quotek60' },
        { name: '日K', val: 'quotekdD' },
        { name: '月K', val: 'quotekdM' },
        { name: '周K', val: 'quotekdK' }
      ],
      acIndex: 0,
      loading: false,
      kType: String,
      datas: [],
      date: [],
      data0: [],
      activeName: 'a',
      myiner: null
    }
  },
  mounted() {
    this.getKC()

    this.drawTime()
  },
  methods: {
    getKC() {
      this.data0 = splitData([
        ['07:00', 2320.26, 2320.26, 2287.03, 2362.94, '+82.27', '+3.06%'],
        ['07:02', 2300.0, 2291.03, 2288.26, 2308.38, '+82.27', '+3.06%'],
        ['07:03', 2295.35, 2346.05, 2295.35, 2346.92, '+82.27', '+3.06%'],
        ['07:04', 2347.22, 2358.98, 2337.35, 2363.08, '+82.27', '+3.06%'],
        ['07:05', 2360.75, 2382.48, 2347.89, 2383.76, '+82.27', '+3.06%'],
        ['07:06', 2383.43, 2385.42, 2371.23, 2391.82, '+82.27', '+3.06%'],
        ['07:07', 2377.41, 2419.02, 2369.57, 2421.15, '+82.27', '+3.06%'],
        ['07:08', 2425.92, 2428.15, 2417.58, 2440.38, '+82.27', '+3.06%'],
        ['07:09', 2411.0, 2433.13, 2403.03, 2437.42, '+82.27', '+3.06%'],
        ['07:10', 2432.68, 2434.48, 2427.07, 2441.73, '+82.27', '+3.06%'],
        ['07:11', 2430.69, 2418.53, 2394.22, 2433.89, '+82.27', '+3.06%'],
        ['07:12', 2416.62, 2432.04, 2414.04, 2443.03, '+82.27', '+3.06%'],
        ['07:13', 2441.91, 2421.56, 2415.43, 2444.08, '+82.27', '+3.06%'],
        ['07:14', 2420.26, 2382.91, 2373.53, 2427.07, '+82.27', '+3.06%'],
        ['07:15', 2383.49, 2397.18, 2370.61, 2397.94, '+82.27', '+3.06%'],
        ['07:16', 2378.82, 2325.95, 2309.17, 2378.82, '+82.27', '+3.06%'],
        ['07:17', 2322.94, 2314.16, 2308.76, 2330.88, '+82.27', '+3.06%'],
        ['07:18', 2320.62, 2325.82, 2315.01, 2338.78, '+82.27', '+3.06%'],
        ['07:19', 2313.74, 2293.34, 2289.89, 2340.71, '+82.27', '+3.06%'],
        ['07:20', 2297.77, 2313.22, 2292.03, 2324.63, '+82.27', '+3.06%'],
        ['07:21', 2322.32, 2365.59, 2308.92, 2366.16, '+82.27', '+3.06%'],
        ['07:22', 2364.54, 2359.51, 2330.86, 2369.65, '+82.27', '+3.06%'],
        ['07:23', 2332.08, 2273.04, 2259.25, 2333.54, '+82.27', '+3.06%'],
        ['07:24', 2274.81, 2326.31, 2270.01, 2328.14, '+82.27', '+3.06%'],
        ['07:25', 2333.61, 2347.18, 2321.06, 2351.44, '+82.27', '+3.06%'],
        ['07:26', 2340.44, 2324.29, 2304.27, 2352.02, '+82.27', '+3.06%'],
        ['07:27', 2326.42, 2318.61, 2314.59, 2333.67, '+82.27', '+3.06%'],
        ['07:28', 2314.68, 2310.59, 2296.58, 2320.96, '+82.27', '+3.06%'],
        ['07:29', 2309.16, 2286.06, 2264.83, 2333.29, '+82.27', '+3.06%'],
        ['07:30', 2282.17, 2263.97, 2253.25, 2286.33, '+82.27', '+3.06%'],
        ['07:31', 2255.77, 2270.28, 2253.31, 2276.22, '+82.27', '+3.06%'],
        ['07:32', 2269.31, 2278.04, 2250.0, 2312.08, '+82.27', '+3.06%'],
        ['07:33', 2267.29, 2240.02, 2239.21, 2276.05, '+82.27', '+3.06%'],
        ['07:34', 2244.26, 2257.43, 2232.02, 2261.31, '+82.27', '+3.06%'],
        ['07:35', 2257.74, 2317.37, 2257.42, 2317.86, '+82.27', '+3.06%'],
        ['07:36', 2318.21, 2324.24, 2311.06, 2330.81, '+82.27', '+3.06%'],
        ['07:37', 2321.04, 2328.28, 2314.97, 2332.0, '+82.27', '+3.06%'],
        ['07:38', 2334.74, 2326.72, 2319.91, 2344.89, '+82.27', '+3.06%'],
        ['07:39', 2318.58, 2297.67, 2281.12, 2319.99, '+82.27', '+3.06%'],
        ['07:40', 2299.38, 2301.26, 2289.0, 2323.48, '+82.27', '+3.06%'],
        ['07:41', 2273.55, 2236.03, 2232.91, 2273.55, '+82.27', '+3.06%'],
        ['07:42', 2238.49, 2236.62, 2228.81, 2246.87, '+82.27', '+3.06%'],
        ['07:43', 2229.46, 2234.04, 2227.31, 2243.95, '+82.27', '+3.06%'],
        ['07:44', 2234.09, 2227.74, 2220.44, 2253.42, '+82.27', '+3.06%'],
        ['07:45', 2232.69, 2225.29, 2217.25, 2241.34, '+82.27', '+3.06%'],
        ['07:46', 2196.24, 2211.59, 2180.67, 2212.59, '+82.27', '+3.06%'],
        ['07:47', 2215.47, 2225.77, 2215.47, 2234.73, '+82.27', '+3.06%'],
        ['07:48', 2224.93, 2226.13, 2212.56, 2233.04, '+82.27', '+3.06%'],
        ['07:49', 2236.98, 2219.55, 2217.26, 2242.48, '+82.27', '+3.06%'],
        ['07:50', 2218.09, 2206.78, 2204.44, 2226.26, '+82.27', '+3.06%'],
        ['07:51', 2199.91, 2181.94, 2177.39, 2204.99, '+82.27', '+3.06%'],
        ['07:52', 2169.63, 2194.85, 2165.78, 2196.43, '+82.27', '+3.06%'],
        ['07:53', 2195.03, 2193.08, 2178.47, 2197.51, '+82.27', '+3.06%'],
        ['07:54', 2281.82, 2197.06, 2175.44, 2206.03, '+82.27', '+3.06%'],
        ['07:55', 2301.12, 2244.64, 2200.58, 2250.11, '+82.27', '+3.06%'],
        ['07:56', 2320.74, 2300.59, 2299.37, 2325.53, '+82.27', '+3.06%'],
        ['07:57', 2264.43, 2242.11, 2240.07, 2266.69, '+82.27', '+3.06%'],
        ['07:58', 2270.71, 2270.93, 2260.87, 2276.86, '+82.27', '+3.06%'],
        ['07:59', 2300.21, 2299.25, 2294.11, 2313.43, '+82.27', '+3.06%'],
        ['08:00', 2242.26, 2210.09, 2205.07, 2250.63, '+82.27', '+3.06%']
      ])

      function splitData(rawData) {
        var categoryData = []
        var values = []
        for (var i = 0; i < rawData.length; i++) {
          categoryData.push(rawData[i].splice(0, 1)[0])
          values.push(rawData[i])
        }
        return {
          categoryData: categoryData,
          values: values
        }
      }

      function calculateMA(dayCount) {
        var result = []
        for (var i = 0, len = data0.values.length; i < len; i++) {
          if (i < dayCount) {
            result.push('-')
            continue
          }
          var sum = 0
          for (var j = 0; j < dayCount; j++) {
            sum += data0.values[i - j][1]
          }
          result.push(sum / dayCount)
        }
        return result
      }

      this.datas = this.data0
    },

    // 绘制分时折线图
    drawTime() {
      var that = this
      var myChart = echarts.init(document.getElementById('main'))
      var arr = this.datas.values
      var yData = []
      var xData = this.datas.categoryData
      var option = {}

      for (var i = 0; i < arr.length; i++) {
        yData.push(arr[i][0])
      }

      option = {
        tooltip: {
          trigger: 'axis',
          axisPointer: {
            type: 'cross',
            label: {
              backgroundColor: 'rgba(255,255,255,1)',
              borderColor: '#ccc',
              borderWidth: '2',
              color: '#000',
              shadowBlur: 0,
              padding: [3, 3],
              fontSize: 13
            },
            crossStyle: {
              type: 'solid'
            }
          },
          showContent: false,
          confine: true
        },
        grid: {
          right: '57px'
        },
        xAxis: {
          type: 'category',
          data: that.data0.categoryData,
          scale1: true,
          splitLine: {
            show: true,
            lineStyle: {
              color: ['#eee'],
              width: 1,
              type: 'solid'
            }
          },
          axisLine: {
            show: false
          },
          axisTick: {
            show: false
          }
        },
        yAxis: {
          type: 'value',
          scale: true,
          boundaryGap: false,
          position: 'right',
          splitLine: {
            show: true,
            lineStyle: {
              color: ['#eee'],
              width: 1,
              type: 'solid'
            }
          },
          axisLine: {
            show: false
          },
          axisTick: {
            show: false
          }
        },
        dataZoom: [
          {
            type: 'inside',
            start: 50,
            end: 100
          },
          {
            show: false,
            type: 'slider',
            x: '90%',
            start: 50,
            end: 100
          }
        ],
        series: [
          {
            name: '数据模拟',
            type: 'line',
            showSymbol: true,
            symbol: 'none',
            sampling: 'average',
            itemStyle: {
              color: '#108EE9'
            },
            lineStyle: {
              width: 1,
              color: '#108EE9'
            },
            areaStyle: {
              color: new echarts.graphic.LinearGradient(0, 0, 0, 1, [
                {
                  offset: 0,
                  color: '#A2D3F7'
                },
                {
                  offset: 1,
                  color: '#FAFDFF'
                }
              ])
            },
            markLine: {
              silent: false,
              symbol: ['none', 'circle'],
              data: [
                {
                  yAxis: yData[yData.length - 1]
                }
              ],
              label: {
                show: true,
                borderColor: '#8EC9F5',
                backgroundColor: '#fff',
                borderWidth: '1',
                precision: 2,
                fontSize: 12,
                padding: [3, 3]
              }
            },
            data: yData
          }
        ]
      }

      this.myiner = setInterval(function() {
        var mess = parseFloat(Math.random() * 200) + 2200
        if (xData[xData.length - 1]) {
          yData[yData.length - 1] = mess
        } else {
          yData.shift()
          yData.push(mess)
        }

        var axisData = (new Date()).toLocaleTimeString().replace(/^\D*/,'');

        option.xAxis.data.shift()
        option.xAxis.data.push(axisData)

        option = {
          tooltip: {
            trigger: 'axis',
            axisPointer: {
              type: 'cross',
              label: {
                backgroundColor: 'rgba(255,255,255,1)',
                borderColor: '#ccc',
                borderWidth: '2',
                color: '#000',
                shadowBlur: 0,
                padding: [3, 3],
                fontSize: 13
              },
              crossStyle: {
                type: 'dashed'
              }
            },
            showContent: false,
            confine: true
          },
          grid: {
            right: '57px'
          },
          xAxis: {
            type: 'category',
            data: that.data0.categoryData,
            scale1: true,
            splitLine: {
              show: true,
              lineStyle: {
                color: ['#eee'],
                width: 1,
                type: 'solid'
              }
            },
            axisLine: {
              show: false
            },
            axisTick: {
              show: false
            }
          },
          yAxis: {
            type: 'value',
            scale: true,
            boundaryGap: false,
            position: 'right',
            splitLine: {
              show: true,
              lineStyle: {
                color: ['#eee'],
                width: 1,
                type: 'solid'
              }
            },
            axisLine: {
              show: false
            },
            axisTick: {
              show: false
            }
          },
          dataZoom: [
            {
              type: 'inside',
              start: 50,
              end: 100
            },
            {
              show: false,
              type: 'slider',
              x: '90%',
              start: 50,
              end: 100
            }
          ],
          series: [
            {
              name: '数据模拟',
              type: 'line',
              showSymbol: true,
              symbol: 'none',
              sampling: 'average',
              itemStyle: {
                color: '#108EE9'
              },
              lineStyle: {
                width: 1,
                color: '#108EE9'
              },
              areaStyle: {
                color: new echarts.graphic.LinearGradient(0, 0, 0, 1, [
                  {
                    offset: 0,
                    color: '#A2D3F7'
                  },
                  {
                    offset: 1,
                    color: '#FAFDFF'
                  }
                ])
              },
              markLine: {
                silent: false,
                symbol: ['none', 'circle'],
                data: [
                  {
                    yAxis: yData[yData.length - 1]
                  }
                ],
                label: {
                  show: true,
                  borderColor: '#8EC9F5',
                  backgroundColor: '#fff',
                  borderWidth: '1',
                  precision: 2,
                  fontSize: 12,
                  padding: [3, 3]
                }
              },
              data: yData
            }
          ]
        }
        myChart.setOption(option)
      }, 2000)
    },

    // 绘制K线图
    drawKline() {
      var that = this
      var myChart = echarts.init(document.getElementById('main'))

      var upColor = '#6CC47C'
      var upBorderColor = '#6CC47C'
      var downColor = '#FD8787'
      var downBorderColor = '#FD8787'

      var option = {
        tooltip: {
          triggerOn: 'mousemove|click',
          trigger: 'axis',
          axisPointer: {
            type: 'cross'
          },
          formatter: function(params) {
            return (
              '时间 <span style="display: inline-block; width: 3px"></span>' +
              '19/05/10 <span style="display: inline-block; width: 3px"></span>' +
              params[0].name +
              '<br>' +
              '价格' +
              '<span style="display: inline-block; width: 58px;"></span>' +
              params[0].value[1] +
              '<br>' +
              '开' +
              '<span style="display: inline-block; width: 70px;"></span>' +
              params[0].value[1] +
              '<br>' +
              '高' +
              '<span style="display: inline-block; width: 70px;"></span>' +
              params[0].value[2] +
              '<br>' +
              '低' +
              '<span style="display: inline-block; width: 70px;"></span>' +
              params[0].value[3] +
              '<br>' +
              '收' +
              '<span style="display: inline-block; width: 70px;"></span>' +
              params[0].value[4] +
              '<br>' +
              '涨跌额' +
              '<span style="display: inline-block; margin-left: 48px; color: #FD8787">' +
              params[0].value[5] +
              '</span><br>' +
              '涨跌幅' +
              '<span style="display: inline-block; margin-left: 42px; color: #FD8787">' +
              params[0].value[6] +
              '</span>'
            )
          },
          showContent: true,
          backgroundColor: 'rgba(255,255,255,1)',
          borderWidth: 1,
          borderColor: 'rgba(221,221,221,1)',
          textStyle: {
            color: '#000'
          },
          padding: [5, 5, 6, 13]
        },
        xAxis: {
          type: 'category',
          data: this.datas.categoryData,
          scale: true,
          boundaryGap: false,
          axisLine: { onZero: false },
          splitLine: { show: false },
          splitNumber: 20,
          min: 'dataMin',
          max: 'dataMax',
          axisLine: {
            show: false
          },
          axisTick: {
            show: false
          },
          splitLine: {
            show: true,
            lineStyle: {
              color: ['#eee'],
              width: 1,
              type: 'solid'
            }
          },
          axisPointer: {
            label: {
              show: true,
              padding: [7, 10],
              borderWidth: 1
            }
          }
        },
        yAxis: {
          scale: true,
          position: 'right',
          axisLine: {
            show: false
          },
          axisTick: {
            show: false
          },
          splitLine: {
            show: true,
            lineStyle: {
              color: ['#eee'],
              width: 1,
              type: 'solid'
            }
          },
          axisPointer: {
            label: {
              show: true,
              padding: [7, 10],
              borderWidth: 1
            }
          }
        },
        dataZoom: [
          {
            type: 'inside',
            start: 50,
            end: 100
          },
          {
            show: false,
            type: 'slider',
            y: '90%',
            start: 50,
            end: 100
          }
        ],
        series: [
          {
            name: '日K',
            type: 'candlestick',
            data: this.datas.values,
            itemStyle: {
              normal: {
                color: upColor,
                color0: downColor,
                borderColor: upBorderColor,
                borderColor0: downBorderColor
              }
            }
          }
        ]
      }

      myChart.setOption(option)
    },

    changeBar(val) {
      this.loading = false
      this.acIndex = val
      if (this.myiner != null) {
        clearInterval(this.myiner)
        this.myiner = null
      }

      switch (val) {
        case 0:
          this.drawTime()
          break
        case 1:
          this.drawKline()
          break
        default:
          break
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="less">
.content {
  .kline {
    width: 100%;
    position: relative;
    height: 300px;
    position: relative;

    .title {
      font-size: 20px;
      font-weight: 500;
      color: rgba(51, 51, 51, 1);
      line-height: 24px;
      position: absolute;
      top: 0;
      left: 31px;

      span {
        color: #fd8787;
        font-weight: normal;
      }
    }

    #main {
      width: 100%;
      height: 300px;
      position: absolute;
      top: 0;
      left: -15px;
    }
  }

  .kbar {
    width: 100%;
    height: 50px;
    line-height: 50px;
    display: flex;
    justify-content: space-around;
    border-bottom: 3px solid #f9f9f9;

    div {
      float: left;
    }
  }

  .active {
    color: #108ee9;
    border-bottom: 2px solid #108ee9;
  }
}
</style>

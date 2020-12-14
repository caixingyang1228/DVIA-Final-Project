<template>
  <div id="app" style="height: 100%; background-color: #f2f2f2">
    <select v-model="dataIndex" style="height: 20px">
      <option value="0">Trigger分享会（夏季）</option>
      <option value="1">Trigger分享会（秋季）</option>
      <option value="2">Trigger分享会（冬季）</option>
      <option value="3">第五次马拉松</option>
      <option value="4">第四次马拉松</option>
      <option value="5">第三次马拉松</option>
      <option value="6">第二次马拉松</option>
      <option value="7">第一次马拉松</option>
    </select>
    <div style="height: calc(100% - 20px); display: flex;">
      <div style="width: 60%; height: 100%; display: flex">
        <div id="map" style="width: 60%; height: 100%"></div>
        <div id="list" style="width: 40%; height: 100%;">
          <div style="height: 100%; display: flex">
            <table style="width: 100%">
              <tr>
                <th>
                  Location
                </th>
                <th>
                  User
                </th>
                <th>
                  Percentage
                </th>
              </tr>
              <tr v-for="(item, index) in data[this.dataIndex].mapData" :key="index">
                <td class="td">{{item.name}}</td>
                <td class="td">{{item.value}}</td>
                <td class="td">{{item.proportion}}</td>
              </tr>
            </table>
          </div>
        </div>
      </div>
      <div style="width: 40%; height: 100%;">
        <div style="text-align: center; font-weight: bold">Demographic breakdown</div>
        <div id="bar" style="width: 100%; height: calc(50% - 21px)"></div>
        <div style="text-align: center; font-weight: bold">Age distribution</div>
        <div id="pie" style="width: 100%; height: calc(50% - 21px)"></div>
      </div>
    </div>
  </div>
</template>

<script>
import echarts from 'echarts'
import china from 'echarts/map/json/china.json'
import data1 from './assets/data1.json'
import data2 from './assets/data2.json'
import data3 from './assets/data3.json'
import data4 from './assets/data4.json'
import data5 from './assets/data5.json'
import data6 from './assets/data6.json'
import data7 from './assets/data7.json'
import data8 from './assets/data8.json'
echarts.registerMap('china', china)
export default {
  name: 'App',
  computed: {
    listTotal: function () {
      let total = 0
      return this.data[this.dataIndex].mapData.reduce((total, item) => total + Number(item.value), 0)
    }
  },
  watch: {
    dataIndex(n, o) {
      this.drawMap()
    }
  },
  data () {
    return {
      data: [data1, data2, data3, data4, data5, data6, data7, data8],
      dataIndex: 0,
      provinces: {
        '上海': '/mapjson/shanghai.json',
        '河北': '/mapjson/hebei.json',
        '山西': '/asset/get/s/data-1591153103530-woA5PfbnV.json',
        '内蒙古': '/mapjson/neimeng.json',
        '辽宁': '/mapjson/liaoning.json',
        '吉林': '/mapjson/jilin.json',
        '黑龙江': '/mapjson/heilongjiang.json',
        '江苏': '/mapjson/jiangsu.json',
        '浙江': '/mapjson/zhejiang.json',
        '安徽': '/mapjson/anhui.json',
        '福建': '/mapjson/fujian.json',
        '江西': '/mapjson/jiangxi.json',
        '山东': '/mapjson/shandong.json',
        '河南': '/mapjson/hainan.json',
        '湖北': '/mapjson/hubei.json',
        '湖南': '/mapjson/hunan.json',
        '广东': '/mapjson/guangdong.json',
        '广西': '/mapjson/guangxi.json',
        '海南': '/mapjson/hainan.json',
        '四川': '/mapjson/sichuan.json',
        '贵州': '/mapjson/guizhou.json',
        '云南': '/mapjson/yunnan.json',
        '西藏': '/mapjson/xizang.json',
        '陕西': '/mapjson/shanxi.json',
        '甘肃': '/mapjson/gansu.json',
        '青海': '/mapjson/qinghai.json',
        '宁夏': '/mapjson/ningxia.json',
        '新疆': '/mapjson/xinjiang.json',
        '北京': '/mapjson/beijing.json',
        '天津': '/mapjson/tianjin.json',
        '重庆': '/mapjson/chongqing.json',
        '香港': '/mapjson/xiangang.json',
        '澳门': '/mapjson/aomen.json'
      }
    }
  },
  mounted() {
    this.drawMap()
  },
  methods: {
    drawMap () {
      let myChart1 = echarts.init(document.getElementById('map'))
      let myChart2 = echarts.init(document.getElementById('bar'))
      let myChart3 = echarts.init(document.getElementById('pie'))
      let option1 = {
        title: {
          text: 'Geographical distribution',
          left: 'left',
          top: '20px'
        },
        tooltip: {
          formatter: function(params) {
            console.log(params)
            return params.name + '<br>' + (params.value ? params.value : 0)
          }
        },
        visualMap: {
          min: 0,
          max: 1000,
          left: 'center',
          top: '90%',
          // seriesIndex: [1],
          inRange: {
            color: ['#ffffff', '#e35e00']
          },
          calculable: true,
          show: true,
          orient: 'horizontal',
          pieces: [
            {value: 1000, label: '1000'},
            {value: 750, label: '750'},
            {value: 500, label: '500'},
            {value: 250, label: '250'},
            {value: 0, label: '0'}
          ],
          textStyle: {

          },
        },
        toolbox: {
          show: true,
          feature: {
            saveAsImage: {
              pixelRatio: 4
            }
          }
        },
        series: [{
          type: 'map',
          mapType: 'china',
          selectedMode: 'false', //是否允许选中多个区域
          label: {
            normal: {
              show: true
            },
            emphasis: {
              show: true
            }
          },
          data: this.data[this.dataIndex].mapData
        }]
      }
      let option2 = {
        series: [
          {
            type: 'pie',
            radius: '65%',
            center: ['50%', '50%'],
            selectedMode: 'single',
            data: this.data[this.dataIndex].age,
            emphasis: {
              itemStyle: {
                shadowBlur: 10,
                shadowOffsetX: 0,
                shadowColor: 'rgba(0, 0, 0, 0.5)'
              }
            }
          }
        ]
      }
      let colorList = ['#73DDFF', '#73ACFF', '#FDD56A', '#FDB36A', '#FD866A', '#9E87FF', '#58D5FF']
      let formatNumber = function(num) {
        let reg = /(?=(\B)(\d{3})+$)/g;
        return num.toString().replace(reg, ',');
      }
      let total = this.data[this.dataIndex].sex.reduce((a, b) => {
        return a + b.value * 1
      }, 0)
      let option3 = {
        title: {
          text: '性别分布\n' + formatNumber(total),
          x: 'center',
          y: 'center',
          textStyle: {
            fontSize: 14
          }
        },
        tooltip: {
          trigger: 'item'
        },
        legend: {
          // orient: 'vertical',
          // top: 'middle',
          bottom: '8px',
          icon: "circle",
          left: 'center',
          data: ['未知', '男', '女']
        },
        series: [{
          type: 'pie',
          center: ['50%', '50%'],
          radius: ['44%', '66%'],
          clockwise: true,
          avoidLabelOverlap: true,
          hoverOffset: 15,
          label: {
            show: true,
            position: 'outside',
            formatter: '{a|{b}：{d}%}\n{hr|}',
            rich: {
              hr: {
                backgroundColor: 't',
                borderRadius: 3,
                width: 3,
                height: 3,
                padding: [3, 3, 0, -12]
              },
              a: {
                padding: [-30, 15, -20, 15]
              }
            }
          },
          labelLine: {
            normal: {
              length: 20,
              length2: 30,
              lineStyle: {
                width: 1
              }
            }
          },
          data: this.data[this.dataIndex].sex
        }]
      }
      myChart1.setOption(option1)
      myChart2.setOption(option2)
      myChart3.setOption(option3)
    }
  }
}
</script>
<style>
  html, body{
    height: 100%;
    margin: 0;
  }
  td {
    text-align: center;
  }
</style>

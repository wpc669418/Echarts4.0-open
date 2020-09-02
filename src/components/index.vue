<template>
  <div class="sales-report">
    <div class="header">电商商品销售趋势图</div>
    <div class="content">
      <div class="content-title-wrapper">月销售增长率</div>
      <div class="content-index-wrapper">
        <span class="percentage">34<span>%</span></span>
        <span class="text">+14,400</span>
      </div>
      <div id="content-chart" />
      <div class="content-circle-wrapper">
        <div
          @click="change(index)"
          :class="['circle', index === selectedIndex ? 'selected' : '']"
          :key="v"
          v-for="(v, index) in circle"
        />
      </div>
      <div class="content-bottom-wrapper">销售趋势</div>
    </div>
    <div class="footer">
      <div class="footer-wrapper">
        <div class="left">
          <div class="footer-title">订单销售额</div>
          <div class="footer-sub-title">3月累计销售额</div>
        </div>
        <div class="right">
          <small>￥</small>
          <span>300,254.00</span>
        </div>
      </div>

      <div class="progress-wrapper">
        <div class="progress-bg">
          <div class="progress-current" :style="`width:${progress * 100}%;`"></div>
        </div>
      </div>

      <div class="footer-text">
          <div>销售增长</div>
          <div>34%</div>
        </div>
    </div>
  </div>
</template>

<script>
import Echarts from 'echarts'

export default {
  name: 'index',
  data: function () {
    return {
      circle: new Array(3),
      selectedIndex: 0,
      progress: 0.34,
      myChart: null
    }
  },
  mounted () {
    const that = this
    window.addEventListener('resize', () => {
      that.myChart.resize()
    })
    this.getChart()
    this.task = setInterval(() => {
      let index = this.selectedIndex
      index++
      if (index >= this.circle.length) {
        index = 0
      }
      this.change(index)
    }, 3000)
  },
  destroyed () {
    if (this.task) {
      clearInterval(this.task)
    }
  },
  methods: {
    change (e) {
      this.selectedIndex = e
      this.getChart()
    },
    getChart () {
      // 获取数据源
      const mockData = []
      for (let i = 0; i < 10; i++) {
        mockData.push(Math.floor(Math.random() * 100) + 200)
      }
      const chartsDom = document.getElementById('content-chart')
      this.myChart = Echarts.init(chartsDom)
      this.myChart.setOption({
        xAxis: {
          type: 'category',
          show: false
        },
        yAxis: {
          min: 0,
          max: 350,
          show: false
        },
        series: [
          {
            type: 'line',
            data: mockData,
            smooth: true,
            areaStyle: {
              color: 'rgba(75,193,252,.5)'
            },
            lineStyle: {
              width: 4,
              color: 'rgba(75,193,252,1)'
            },
            itemStyle: {
              borderWidth: 8,
              color: 'rgba(75,193,252,1)'
            }
          }
        ],
        grid: {
          top: 0,
          bottom: 0,
          left: -30,
          right: -30
        },
        tooltip: {
          trigger: 'axis',
          axisPointer: {
            type: 'cross',
            label: {
              backgroundColor: '#6a7985'
            }
          }
        }
      })
    }
  }
}
</script>

<style lang="scss" secoped>
#charts {
  width: 200px;
  height: 200px;
  background: red;
}
.sales-report {
  width: 100%;
  height: 100%;
  background: #fff;
  box-shadow: 0 2px 8px rgba(4, 9, 20, 0.06),
    0 2px 8px rgba(4, 9, 20, 0.06) ，0 2px 8px rgba(4, 9, 20, 0.06) ，0 2px 8px
      rgba(4, 9, 20, 0.06) ，;
  display: flex;
  flex-direction: column;
  .header {
    width: 100%;
    // height: 50px;
    padding: 14px 0 14px 28px;
    align-items: center;
    border-bottom: 1px solid #eee;
    box-sizing: border-box;
    color: rgba(13, 27, 62, 0.7);
  }
  .content {
    flex: 1;
    width: 100%;
    padding: 0 28px;
    display: flex;
    flex-direction: column;
    box-sizing: border-box;
    .content-title-wrapper {
      padding-top: 28px;
      font-size: 13px;
      color: rgb(102, 117, 125);
    }
    .content-index-wrapper {
      display: flex;
      align-items: center;
      margin-top: 15px;
      .percentage {
        font-size: 35px;
        font-weight: 700;
        color: #333;
        span {
          font-size: 28px;
          margin-left: 5px;
          color: #eee;
        }
      }
      .text {
        color: rgb(58, 196, 125);
        margin-left: 15px;
      }
    }
    #content-chart {
      flex: 1;
    }
    .content-circle-wrapper {
      display: flex;
      justify-content: center;
      align-items: center;
      margin-top: 20px;
      .circle {
        width: 10px;
        height: 10px;
        background: #fff;
        border: 3px solid rgb(63, 106, 216);
        border-radius: 50%;
        margin: 0 5px;
        cursor: pointer;
        &.selected {
          width: 12px;
          height: 12px;
          border: 5px solid rgb(63, 106, 216);
        }
      }
    }
    .content-bottom-wrapper {
      margin: 10px 0;
      color: #999;
    }
  }
  .footer {
    width: 100%;
    height: 120px;
    box-sizing: border-box;
    padding: 0 30px;
    display: flex;
    flex-direction: column;
    .progress-wrapper{
      .progress-bg{
        height: 10px;
        width: 100%;
        border-radius: 3.5px;
        background: #eee;
        position: relative;
      }
      .progress-current{
        height: 100%;
        width: 35%;
        border-radius: 3.5px;
        background: #3f6ad8;
        position: relative;
        &::after{
          content: '';
          position: absolute;
          top:0;
          left: 0;
          width: 100%;
          background: #fff;
          height: 100%;
          opacity: 0.5;
          animation: process-active 2s ease infinite;
        }
        @keyframes process-active {
          from{
            width:0;
            opacity: 0;
          }
          to{
            width: 100%;
            opacity: 0.3;
          }
        }
      }
    }
    .footer-wrapper {
      display: flex;
      justify-content: space-between;
      align-items: center;
      box-sizing: border-box;
      padding: 14px 0;
      .left{
        .footer-title{
          font-size: 13px;
          font-weight: 700;
          color: #333;
        }
        .footer-sub-title{
          font-size: 13px;
          color: #999;
        }
      }
      .right{
        flex:1;
        text-align: right;
        color: rgb(58,196,125);
        font-size: 25px;
        font-weight: 700;
        small{
          font-weight: 400;
        }
      }
    }
    .footer-text{
      display: flex;
      justify-content: space-between;
      font-size: 13px;
      color: #999;
      padding: 14px 0;
    }
  }
}
</style>

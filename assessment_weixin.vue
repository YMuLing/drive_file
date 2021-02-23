<template style="height: 100%">
  <view>
    <!-- 导航区域 -->
    <view class="nav">
      <view class="top" :style="statusHeight"></view>
      <view class="title">
        <navigator
          url="/pages/drive/index"
          open-type="switchTab"
          hover-class="none"
        >
          <image src="/static/icons/prev.png" mode="" />
        </navigator>
        <text>考核指标</text>
      </view>
    </view>
    <!-- 导航占位 -->
    <view class="navposition" :style="navHeight"></view>
    <!-- 主体内容 -->
    <view class="main">
      <!-- 数据展示 -->
      <view class="business">
        <view class="title">存款业务</view>

        <view class="business_cont">
          <view class="sameday">
            <text class="item_title">同日存款增幅</text>
            <view class="item_cont_num">
              <text>{{ businessData.sameDay }}</text
              ><text>{{ businessData.average }}</text>
            </view>
            <view class="item_cont">
              <text>平均值</text><text>最高值</text>
            </view>
          </view>
          <view class="line"></view>
          <view class="average">
            <text class="item_title">日均存款增幅</text>
            <view class="item_cont_num">
              <text>9.42%</text><text>9.42%</text>
            </view>
            <view class="item_cont">
              <text>平均值</text><text>最高值</text>
            </view>
          </view>
        </view>
      </view>
      <!-- 数据展示 -->
      <view class="business">
        <view class="title">贷款业务</view>
        <view class="business_cont">
          <view class="sameday">
            <text class="item_title">同日存款增幅</text>
            <view class="item_cont_num">
              <text>{{ businessData.sameDay }}</text
              ><text>{{ businessData.average }}</text>
            </view>
            <view class="item_cont">
              <text>平均值</text><text>最高值</text>
            </view>
          </view>
          <view class="line"></view>
          <view class="average">
            <text class="item_title">日均存款增幅</text>
            <view class="item_cont_num">
              <text>9.42%</text><text>9.42%</text>
            </view>
            <view class="item_cont">
              <text>平均值</text><text>最高值</text>
            </view>
          </view>
        </view>
      </view>

      <view class="charts">
        <text class="charts_title">各项贷款占比</text>
        <view class="charts_cont" id="doma"> </view>
      </view>
    </view>
    <view class="charts_cont">
      <view id="container" style="height: 220px"></view>
      <view class="name">
        <view>
          <text> 贷款1</text>
          <text>xxxx贷款3</text>
        </view>
        <view>
          <text>xxx贷款2</text>
          <text>xxxx贷款3</text>
        </view>
      </view>
      <view class="line"></view>
    </view>
  </view>
</template>

<script>
let echarts = require("../../../static/js/echarts.js");
console.log(echarts);
export default {
  data() {
    return {
      // 状态栏高度
      statusHeight: "",
      // 占位高度
      navHeight: "",

      // 业务数据
      businessData: {
        sameDay: "9.42%",
        average: "10.21%",
      },
    };
  },
  created() {
    // 获取原型对象上的设备高度，需要先在 app.vue,挂载
    this.statusHeight = this.$statuHeight;
    this.navHeight = this.$navHeight;
  },
  mounted: function () {
    this.$nextTick(function () {
      var dom = document.getElementById("container");
      var myChart = echarts.init(dom);
      var myChart = echarts.init(dom);
      var app = {};

      var option;

      option = {
        color: ["#03E6DA", "#02B1FF", "#6363FF", "#1B79FF"],
        legend: {
          top: "bottom",
        },

        series: [
          {
            name: "半径模式",
            type: "pie",
            roseType: "radius",
            radius: [40, 100],
            center: ["50%", "50%"],
            roseType: "area",
            labelLine: {
              normal: {
                position: "inner",
                show: false,
              },
            },
            itemStyle: {
              borderRadius: 4,
            },
            data: [{ value: 50 }, { value: 40 }, { value: 30 }, { value: 20 }],
          },
        ],
      };

      myChart.setOption(option);
    });
  },
  methods: {},
};
</script>

<style lang="scss" scoped>
html,
body {
  height: 100%;
}

// 导航区
.nav {
  position: fixed;
  width: 100%;
  top: 0;
  left: 0;
  z-index: 4;
  .top {
    width: 100%;
    background-color: #fff;
  }
  .title {
    position: relative;
    width: 100%;
    height: 88rpx;
    display: flex;
    align-items: center;
    background-color: #fff;
    border-bottom: 1rpx solid #ededed;
    .className {
      background-color: transparent;
    }
    navigator {
      width: 52rpx;
      height: 52rpx;
      position: absolute;
      top: 50%;
      left: 30rpx;
      transform: translateY(-50%);
      margin-top: 4rpx;
    }
    image {
      width: 32rpx;
      height: 32rpx;
    }
    text {
      width: 100%;
      height: 100%;
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 34rpx;
      color: #333;
    }
  }
}

// 主体内容
.main {
  padding: 30rpx 30rpx 0;
  .business {
    margin-bottom: 30rpx;
    padding: 20rpx 20rpx 28rpx;
    display: flex;
    flex-direction: column;
    background-color: #fff;
    .title {
      margin-bottom: 18rpx;
      font-size: 32rpx;
      font-weight: 700;
      color: #000;
    }
    .business_cont {
      position: relative;
      display: flex;
      justify-content: space-between;
      .line {
        position: absolute;
        left: 50%;
        width: 1rpx;
        height: 132rpx;
        background-color: #ededed;
      }
      .average {
        margin-left: 30rpx;
      }
      .sameday,
      .average {
        .item_title {
          font-size: 24rpx;
          color: #999999;
        }
        .item_cont_num {
          margin: 16rpx 0 9rpx;
          font-size: 32rpx;
          font-weight: 700;
          width: 100%;
          text {
            margin-right: 63rpx;
          }
        }
        .item_cont {
          font-size: 22rpx;
          color: #999999;
        }
      }
    }
  }
  .charts {
    padding: 20rpx 20rpx 28rpx;
    background-color: #fff;
    .charts_title {
      margin-bottom: 18rpx;
      font-size: 32rpx;
      font-weight: 700;
      color: #000;
    }
  }
}

.charts_cont {
  padding: 0 30rpx;
  overflow: hidden;
  .name {
    display: flex;
    flex-direction: column;
    align-items: center;
    background-color: #fff;
    view {
      width: 386rpx;
      display: flex;
      justify-content: space-between;
      text {
        font-size: 22rpx;
        color: #333;
        &::before {
          content: "";
          display: inline-block;
          width: 16rpx;
          height: 16rpx;
          margin: -4rpx 6rpx 0 0;
          vertical-align: middle;
          border: 6rpx solid #333;
          border-radius: 50%;
        }
      }
    }
    view:nth-child(1) text:nth-child(1)::before {
      border-color: #02b1ff;
    }
    view:nth-child(1) text:nth-child(2)::before {
      border-color: #6363ff;
    }
    view:nth-child(2) text:nth-child(1)::before {
      border-color: #05e5da;
    }
    view:nth-child(2) text:nth-child(2)::before {
      border-color: #d792fa;
    }
  }
}
#container {
  background-color: #fff;
}
.line {
  height: 30rpx;
  background-color: #fff;
}
</style>
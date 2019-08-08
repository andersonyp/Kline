<template>
  <div class="content">
    <div class="header"></div>
    <div class="nav">
      <div class="nav-left">
        <li>
          开盘
          <span>3975.86</span>
        </li>
        <li>
          最高
          <span style="color: #FD8787">3975.86</span>
        </li>
        <li>
          收盘
          <span>3975.86</span>
        </li>
        <li>
          最低
          <span style="color: #6CC47C">3975.86</span>
        </li>
      </div>
      <div class="nav-right">
        <p>+2.05%</p>
        <div class="triangle"></div>
      </div>
    </div>

    <Kecharts></Kecharts>

    <div class="usdt">
      <div class="u-left">
        可用
        <span>54356.00 USDT</span>
      </div>
      <div class="u-right">
        占用
        <span>3423.00 USDT</span>
      </div>
    </div>

    <div class="sale">
      <div class="sale-in">
        3993.
        <span style="font-size: 20px">18</span> 卖出
      </div>
      <div class="circle">10</div>
      <div class="sale-out">
        3993.
        <span style="font-size: 20px">28</span> 买入
      </div>
    </div>
  </div>
</template>

<script>
import Kecharts from 'common/Kecharts'
export default {
  components: {
    Kecharts
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="less">
.content {
  .header {
    height: 40px;
    background-color: #d81e06;
  }

  .nav {
    height: 110px;
    overflow: hidden;
    position: relative;

    .nav-left {
      width: 240px;
      height: 50px;
      position: absolute;
      bottom: 10px;
      left: 25px;

      li {
        list-style: none;
        float: left;
        width: 120px;
        height: 30px;
        line-height: 30px;
        font-size: 18px;
        color: #7d7d7d;

        span {
          color: #333333;
        }
      }
    }

    .nav-right {
      position: absolute;
      right: 50px;
      bottom: 0;

      p {
        font-size: 20px;
        color: #fd8787;
      }

      .triangle {
        width: 0;
        height: 0;
        border-left: 7px solid transparent;
        border-right: 7px solid transparent;
        border-bottom: 10px solid #ff6161;
        position: absolute;
        right: -25px;
        bottom: 28px;
      }
    }
  }

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

  .usdt {
    height: 70px;
    width: 100%;
    display: flex;

    div {
      height: 70px;
      line-height: 70px;
      flex: 1;
      text-align: center;
      font-size: 18px;
      color: #7e88a2;

      span {
        color: #333333;
        margin-left: 5px;
      }
    }
  }

  .sale {
    height: 116px;
    width: 100%;
    position: relative;
    background-color: #f9f9f9;

    div {
      width: 142px;
      height: 40px;
      border-radius: 5px;
      position: absolute;
      top: 50%;
      transform: translateY(-50%);
      line-height: 40px;
      text-align: center;
      font-size: 16px;
      font-weight: 600;
      color: #fff;
    }

    .circle {
      width: 36px;
      height: 36px;
      background-color: #fff;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      border-radius: 18px;
      font-size: 16px;
      font-weight: 600;
      text-align: center;
      line-height: 36px;
      color: #7e88a2;
      border: 1px solid #dddddd;
    }

    .sale-in {
      background-color: #6cc47c;
      left: 17px;
    }

    .sale-out {
      right: 17px;
      background-color: #fd8787;
    }
  }

  .active {
    color: #108ee9;
    border-bottom: 2px solid #108ee9;
  }
}
</style>

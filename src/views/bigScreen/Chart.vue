<template>
  <div class="ChartWrapper">
    <div class="ChartWrapper-top">
      <div class=ChartWrapper-top-title>零件故障率</div>

      <a-radio-group size="large" default-value="a" button-style="solid">
        <a-radio-button value="a">
          本日
        </a-radio-button>
        <a-radio-button value="b">
          本月
        </a-radio-button>
        <a-radio-button value="c">
          本年
        </a-radio-button>
      </a-radio-group>
    </div>



    <div class="ChartBox">
      <div class="chart">
        <Pie radius="1" :height="320" :showLegend="false" :pie-style="pieStyle" :color="color" :data-source="dataSource" inner-radius="0.6"></Pie>
      </div>

      <div class="count-data">
        <div>
          <div class="count-data-label">合格率</div>
          <div class="count-data-value">10%</div>
        </div>
        <div>
          <div class="count-data-label">检测数量</div>
          <div class="count-data-value">10%</div>
        </div>
        <div>
          <div class="count-data-label">故障次数</div>
          <div class="count-data-value">10%</div>
        </div>
      </div>
    </div>
    <div class="ChartWrapper-search">
      <span class="ChartWrapper-search-label">搜索：</span>
      <a-input class="ChartWrapper-search-input" size="large" placeholder="请输入零件图号查询"></a-input>
      <a-button size="large"  type="primary">搜索</a-button>
      <a-button size="large">重置</a-button>
    </div>

  </div>
</template>

<script>
import Pie from '@comp/chart/Pie'
const color = ['item', ['#05B3FF', '#FE7036FF', '#fbce1e', '#2b3b79', '#8a4be2', '#1dc5c5']];

export default {
  name: 'Chart',
  components: {Pie},
  data() {
    return {
      height: 300,
      color,
      dataSource: [
        { item: '故障', count: 40 },
        { item: '示例二', count: 21 },
      ],
      pieStyle: {
        stroke: '#1273DB',
        lineWidth: 0
      }
    }
  }
}
</script>

<style lang="less" scoped>
.ChartWrapper {
  width: 100%;
  padding: 38px 40px 38px 0;
  border-radius: 12px;
  background: rgba(5, 13, 75, 1);
  border: 2px solid rgba(8, 72, 138, 1);
  box-shadow:inset 0px 1px 20px 0px rgba(18, 142, 232, 0.34);

  &-top {
    padding-left: 38px;
    display: flex;
    justify-content: space-between;

    &-title {
      font-size: 24px;
      font-weight: 700;
    }
  }

  .ChartBox {
    display: flex;
    //justify-content: space-between;


    .chart {
      margin-top: 40px;
      width: 400px;
    }

    .count-data {
      padding-left: 40px;
      margin-top: 45px;

      &-label {
        font-size: 16px;
        color: rgba(133, 144, 179, 1);
      }

      &-value {
        font-size: 36px;
        font-weight: 700;
        color: rgba(254, 124, 75, 1);
      }
    }
  }

  &-search {
    padding-left: 38px;
    margin-top: -64px;
    display: flex;
    align-items: center;

    &-label {
      font-size: 16px;
      font-weight: 400;
      color: rgba(255, 255, 255, 0.8);
    }

    button {
      margin-left: 20px;
      width: 80px;
    }

    &-input {
      width: 289px;
      border-radius: 10px;
      background: rgba(0, 0, 0, 0.2);
      border: 1px solid rgba(255, 255, 255, 0.2);
      color: #fff;

      &::placeholder  {
        color: rgba(255, 255, 255, 0.5);
      }
    }
  }


  /deep/ .ant-radio-button-wrapper {
    background: rgba(0, 0, 0, 0.2);
    color: rgba(255, 255, 255, 0.7);
    border-color:  rgba(255, 255, 255, 0.2);
  }

  /deep/ .ant-radio-button-wrapper:not(:first-child)::before {
    background-color:  transparent;
  }

  /deep/ .ant-radio-group-solid .ant-radio-button-wrapper-checked:not(.ant-radio-button-wrapper-disabled) {
    background: rgba(18, 115, 219, 1);
    border-color: rgba(18, 115, 219, 1);
    color: #fff;
  }
}

.ChartBox {

}
</style>
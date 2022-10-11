<template>
  <div class="ChartWrapper">
    <div class="ChartWrapper-top">
      <div class="ChartWrapper-top-title">零件合格率</div>

      <a-radio-group size="large" default-value="1" @change="onDateChangne" button-style="solid">
        <a-radio-button value="1">
          本日
        </a-radio-button>
        <a-radio-button value="2">
          本月
        </a-radio-button>
        <a-radio-button value="3">
          本年
        </a-radio-button>
      </a-radio-group>
    </div>


    <div class="ChartBox">
      <div class="chart">
        <template v-if="!data.pass && !data.failure">
          <div class="empty-chart">
            <a-empty :image="simpleImage" />
          </div>
        </template>
        <Pie
          v-else
          :radius="0.9"
          :height="320"
          :showLegend="false"
          :pie-style="pieStyle"
          :color="color"
          :data-source="dataSource"
          :labelConfig="labelConfig"
          inner-radius="0.6"></Pie>
      </div>

      <div class="count-data">
        <div>
          <div class="count-data-label">合格率</div>
          <div class="count-data-value">{{ data.passRate }}%</div>
        </div>
        <div>
          <div class="count-data-label">检测数量</div>
          <div class="count-data-value">{{ data.checked }}</div>
        </div>
        <div>
          <div class="count-data-label">故障次数</div>
          <div class="count-data-value">{{ data.failure }}</div>
        </div>
      </div>
    </div>
    <div class="ChartWrapper-search">
      <span class="ChartWrapper-search-label">搜索：</span>
      <a-input class="ChartWrapper-search-input" v-model="params.productDraw" size="large" placeholder="请输入零件图号查询"></a-input>
      <a-button @click="search" size="large" type="primary">搜索</a-button>
      <a-button @click="reset" size="large">重置</a-button>
    </div>

  </div>
</template>

<script>
import { Empty } from 'ant-design-vue';
import { getBigScreenFailureRate } from '@/api/api'
import Pie from '@comp/chart/Pie'
// 'l(270) 0:#4037ED 1:#FE7036'
const color = ['item', ['#FE7036', 'l(270) 0:#05B3FF 1:#4037ED', '#fbce1e', '#2b3b79', '#8a4be2', '#1dc5c5']]

export default {
  name: 'Chart',
  components: { Pie },
  data() {
    return {
      height: 300,
      color,
      simpleImage: '',
      data: { checked: 0, pass: 0, failure: 0, passRate: 0 },
      pieStyle: {
        stroke: '#1273DB',
        lineWidth: 0
      },
      params: {
        type: '1',
        productDraw: '',
      },
      labelConfig: [
        'percent',
        (x,a) => ({
          formatter(val, item) {
            if (item.point.item !== '不合格') return '';
            if (item.point.count === 0) return '';
            return item.point.item;
          },
          textStyle: {
            fill: a.color,
            fontSize: 20,
          }
        })
      ],

    }
  },
  computed: {

    dataSource() {
      return [
        { item: '不合格', count: this.data.failure || 0 },
        { item: '合格', count: this.data.pass }
      ]
    }
  },
  beforeCreate() {
    this.simpleImage = Empty.PRESENTED_IMAGE_SIMPLE;
  },
  mounted() {
    this.getData()
  },
  methods: {
    async getData() {
      const data = await getBigScreenFailureRate({
        ...this.params
      })
      console.log(data)
      this.data = data || {checked: 0, pass: 0, failure: 0, passRate: 0};
      // this.data = {checked: 10, pass: 10, failure: 0, passRate: 100};
    },
    onDateChangne(event) {
      this.params.type = event.target.value
      this.getData()
    },
    search() {
      this.getData();
    },
    reset() {
      this.params.productDraw = '';
      this.getData();
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
  box-shadow: inset 0px 1px 20px 0px rgba(18, 142, 232, 0.34);

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
    height: 365px;
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

      &::placeholder {
        color: rgba(255, 255, 255, 0.5);
      }
    }
  }

  .empty-chart{
    //width: 400px;
    //height: 366px;
  }


  /deep/ .ant-radio-button-wrapper {
    background: rgba(0, 0, 0, 0.2);
    color: rgba(255, 255, 255, 0.7);
    border-color: rgba(255, 255, 255, 0.2);
  }

  /deep/ .ant-radio-button-wrapper:not(:first-child)::before {
    background-color: transparent;
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
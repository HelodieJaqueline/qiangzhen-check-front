<template>
  <v-chart :forceFit="true" :height="height" :data="data" :scale="scale" :onClick="handleClick">
    <v-tooltip :showTitle="false" dataKey="item*percent"/>
    <v-axis/>
    <v-legend v-if="showLegend" dataKey="item"/>
    <v-pie position="percent" :color="color" :v-style="pieStyle" :label="labelConfig"/>
    <v-coord type="theta"  :radius="radius" :innerRadius="innerRadius" />
  </v-chart>
</template>

<script>
  const DataSet = require('@antv/data-set')
  import { ChartEventMixins } from './mixins/ChartMixins'

  export default {
    name: 'Pie',
    mixins: [ChartEventMixins],
    props: {
      title: {
        type: String,
        default: ''
      },
      showLegend: {
        type: Boolean,
        default: true,
      },
      height: {
        type: Number,
        default: 254
      },
      color: {
        type: [Array, String],
        default: () => 'item'
      },
      radius: {
        type: Number,
        default: 1
      },
      innerRadius: {
        type: Number,
        default: 0
      },
      dataSource: {
        type: Array,
        default: () => [
          { item: '示例一', count: 40 },
          { item: '示例二', count: 21 },
          { item: '示例三', count: 17 },
          { item: '示例四', count: 13 },
          { item: '示例五', count: 9 }
        ]
      },
      pieStyle: {
        type: Object,
        default: () => ({
          stroke: '#fff',
          lineWidth: 1
        })
      },
      formatter: {
        type: Function,
        default(val, item) {
          return item.point.item + ': ' + val
        }
      }
    },
    data() {
      return {
        scale: [{
          dataKey: 'percent',
          min: 0,
          formatter: '.0%'
        }],
        // pieStyle: {
        //   stroke: '#fff',
        //   lineWidth: 1
        // },
        labelConfig: ['percent', {
          formatter: (val, item) => this.formatter(val, item),
        }]
      }
    },
    computed: {
      data() {
        let dv = new DataSet.View().source(this.dataSource)
        // 计算数据百分比
        dv.transform({
          type: 'percent',
          field: 'count',
          dimension: 'item',
          as: 'percent'
        })
        return dv.rows
      }
    }
  }
</script>
<template>
  <div class="summary">
    <a-row :gutter="100">
      <a-col :span="10">
        <div class="summary-title" style="margin-bottom: 30px;">数据总览</div>

        <a-row :gutter="20">
          <a-col
            v-for="item of summaryList"
            span="12"
          >
            <div class="summary-item">
              <div class="summary-item-label">{{ item.label }}</div>
              <div class="summary-item-value">{{ item.render ? item.render(summary[item.key]) : summary[item.key] }}</div>
            </div>
          </a-col>
        </a-row>
        <a-radio-group size="large" default-value="1" @change="onSummaryTypeChange" button-style="solid">
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
      </a-col>
      <a-col :span="14">
        <div class="summary-title" style="text-align: center;">送检合格率排行榜</div>
        <vue-seamless-scroll :data="list" :classOption="classOption" class="check-rank-wrap" ref="seamlessScroll">
          <div class="check-rank" v-for="(item, index) of list">
            <span style="min-width: 20px; text-align: center;">
              <template v-if="index < 3">
                <img :src="require(`@/assets/bigScreen/${index + 1}.png`)" alt="">
              </template>
              <template v-else>
                {{ index + 1 }}
              </template>
            </span>
            <span style="min-width: 158px;">{{ item.materialName }}</span>
            <span style="min-width: 174px;">{{ item.productDraw }}</span>
            <span style="min-width: 50px;">{{ item.rate }}</span>
          </div>
        </vue-seamless-scroll>
      </a-col>
    </a-row>
  </div>
</template>

<script>
import vueSeamlessScroll from 'vue-seamless-scroll'
import { getBigScreenSummary, getBigScreenPassRate } from '@/api/api'

export default {
  name: 'Summary',
  components: {
    vueSeamlessScroll
  },
  data() {
    return {
      classOption: {
        singleHeight: 60,
        autoPlay: false,
      },

      summaryType: '1',
      summaryList: [
        {
          label: '总检测数量',
          key: 'total'
        }, {
          label: '待检测数量',
          key: 'uncheck'
        }, {
          label: '总合格率',
          key: 'totalPassRate',
          render(val) {
            return val + '%'
          }
        }, {
          label: '在检测数量',
          key: 'checking',
        }
      ],
      summary: {
        total: '0',
        uncheck: '0',
        totalPassRate: '0',
        checking: '0'
      },
      list: [

      ]
    }
  },
  mounted() {
    this.getSummary()
    this.getList();
  },
  beforeDestroy() {
    clearTimeout(this.getListTimer)
    clearTimeout(this.getSummaryTimer)
  },
  methods: {
    onSummaryTypeChange(event) {
      console.log(event)
      this.summaryType = event.target.value;
      this.getSummary();
    },
    async getSummary() {
      try {
        this.summary = await getBigScreenSummary({
          type: this.summaryType
        })
      } catch (err) {
        console.error(err)
      }
      this.getSummaryTimer = setTimeout(this.getSummary, 5000)
    },

    async getList()  {
      try {
        this.list = await getBigScreenPassRate();
        this.classOption.autoPlay = this.list.length > 5;
        this.$refs.seamlessScroll.reset();
      } catch (err) {

      }
      this.getListTimer = setTimeout(this.getList, 5000)
    }
  }
}
</script>

<style lang="less" scoped>
.summary {
  width: 100%;
  //min-width: 1100px;
  padding: 38px 40px;
  border-radius: 12px;
  background: rgba(5, 13, 75, 1);
  border: 2px solid rgba(8, 72, 138, 1);
  box-shadow: inset 0 1px 20px 0 rgba(18, 142, 232, 0.34);

  &-title {
    font-size: 24px;
  }

  &-item {
    margin-bottom: 20px;
    padding: 20px 28px;
    border-radius: 10px;
    background: linear-gradient(135deg, rgba(5, 179, 255, 1) 0%, rgba(0, 126, 230, 1) 100%);

    &-label {
      font-size: 16px;
    }

    &-value {
      font-size: 36px;
      font-weight: 700;
      letter-spacing: 1px;
    }
  }

  .check-rank-wrap {
    margin-top: 40px;
    height: 300px;
    overflow: hidden;
  }

  .check-rank {
    display: flex;
    line-height: 21px;
    font-size: 18px;

    span {
      flex: 1;
      line-height: 60px;
      padding: 0 15px;
      white-space: nowrap;
      overflow: hidden;
      text-overflow: ellipsis;
    }
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
</style>
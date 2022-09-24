<template>
  <div>
    <div class="screen-table-header">
      <div class="screen-table-header-item" :style="{minWidth: item.width + 'px'}" v-for="item of columns">
        {{item.title}}
      </div>
    </div>
    <vue-seamless-scroll :data="dataSource" :classOption="classOption" class="screen-table-body" ref="seamlessScroll">
      <div class="screen-table-row" v-for="(item, index) of dataSource">
        <div class="screen-table-col" :style="{minWidth: column.width + 'px'}" v-for="column of columns">
          <template
           v-if="column.key === 'action'"
          >
            <span @click="handleDetail(item)" class="detailBtn">详情</span>
          </template>
          <template
           v-else-if="column.dataIndex === 'checkStatus'"
          >
            <span style="color: #FE7C4B;">{{statusRender(item.checkStatus)}}</span>
          </template>
          <template
            v-else-if="column.dataIndex === 'qualifiedStatus'"
          >
            <span :style="{color: qualifiedStatusColor(item.qualifiedStatus)}">
              {{qualifiedStatusRender(item.qualifiedStatus)}}
            </span>
          </template>
          <template
            v-else-if="column.dataIndex === 'evaluateTime'"
          >
            <span>
              {{item.evaluateTime ? `${item.evaluateTime}H` : '/'}}
            </span>
          </template>
          <template v-else>
            {{item[column.dataIndex] || '/'}}
          </template>
        </div>
      </div>
    </vue-seamless-scroll>
    <qzqm-unchecked-info-modal ref="modalForm"></qzqm-unchecked-info-modal>
  </div>
</template>
<script>
import vueSeamlessScroll from 'vue-seamless-scroll';
import { getBigScreenTable } from '@/api/api';
import QzqmUncheckedInfoModal from '@views/check/modules/QzqmUncheckedInfoModal'
const columns = [
  {
    dataIndex: 'id',
    key: 'id',
    title: '检测编号',
    width: 82,
  },
  {
    title: '零件图号',
    dataIndex: 'productDraw',
    key: 'productDraw',
    width: 164,
  },
  {
    width: 164,
    title: '物料号',
    dataIndex: 'materialCode',
    key: 'materialCode'
  },
  {
    width: 164,
    title: '零件名称',
    key: 'materialName',
    dataIndex: 'materialName',
    scopedSlots: { customRender: 'tags' }
  }, {
    width: 80,
    title: '检测状态',
    key: 'checkStatus',
    dataIndex: 'checkStatus',
    // scopedSlots: { customRender: 'tags' }
  }, {
    width: 90,
    title: '合格状态',
    key: 'qualifiedStatus',
    dataIndex: 'qualifiedStatus',
  }, {
    width: 148,
    title: '送检时间',
    key: 'deliveryTime',
    dataIndex: 'deliveryTime',
  }, {
    width: 120,
    title: '预估完成时间',
    key: 'evaluateTime',
    dataIndex: 'evaluateTime',
  }, {
    width: 148,
    title: '结束时间',
    key: 'finishedTime',
    dataIndex: 'finishedTime',
    scopedSlots: { customRender: 'tags' }
  }, {
    width: 80,
    title: '送检人',
    key: 'deliveryUserName',
    dataIndex: 'deliveryUserName',
  }, {
    width: 80,
    title: '送检部门',
    key: 'deliveryDep',
    dataIndex: 'deliveryDep',
  }, {
    width: 84,
    title: '送检工序',
    key: 'process',
    dataIndex: 'process',
  },
  {
    title: '操作',
    key: 'action',
    width: 40,
  }
]

export default {
  components: {
    vueSeamlessScroll,
    QzqmUncheckedInfoModal
  },
  data() {
    return {
      dataSource: [],
      classOption: {
        singleHeight: 54
      },
      columns,
      pageNum: 1,
    }
  },
  mounted() {
    this.getData();
  },
  beforeDestroy() {
    clearTimeout(this.timer)
  },
  methods: {
    handleDetail:function(record){
      this.$refs.modalForm.edit(record);
      this.$refs.modalForm.title="详情";
      this.$refs.modalForm.disableSubmit = true;
    },
    statusRender(status) {
      return ['待检测', '检测中',  '检测完成'][status] || '/'
    },
    qualifiedStatusRender(status) {
      return ['/', '合格',  '不合格'][status] || '/'
    },
    qualifiedStatusColor(status) {
      return ['#fff', '#1AC4A2',  '#F04175'][status] || '#fff'
    },
    async getData() {
      try {
        const { result } = await getBigScreenTable({
          pageNum: this.pageNum,
          pageSize: 10,
        });
        const { current, records } = result;
        this.dataSource = records;
        this.$refs.seamlessScroll.reset();
      } catch (err) {

      }
      this.timer = setTimeout(this.getData, 30000)
    }
  }
}
</script>

<style  lang="less" scoped>
.screen-table-header {
  display: flex;
  font-size: 15px;
  color: rgba(255, 255, 255, 0.7);
  padding: 14px;
  background: rgba(18, 115, 219, 1);

  &-item {
    flex: 1;
    text-align: center;
    border-left: 1px solid  rgba(255, 255, 255, 0.2);

    &:first-of-type {
      border-left: none;
    }
  }
}
.screen-table-body {
  height: 324px;
  overflow: hidden;

  .detailBtn {
    display: inline-block;
    width: 52px;
    height: 24px;
    line-height: 24px;
    //opacity: 0.15;
    border-radius: 2px;
    background: #1273DB;
    cursor: pointer;
  }
}

.screen-table-row {
  display: flex;
}

.screen-table-col  {
  flex: 1;
  height: 54px;
  line-height: 54px;
  border-bottom: 1px solid rgba(255, 255, 255, 0.05);
  text-align: center;
  color: #fff;
  font-size: 15px;
  letter-spacing: 0;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}
</style>

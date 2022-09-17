<template>
  <a-card :bordered="false">
    <!-- 查询区域 -->
    <div class="table-page-search-wrapper">
      <a-form layout="inline" @keyup.enter.native="searchQuery">
        <a-row :gutter="24">
          <a-col :md="6" :sm="10">
            <a-form-item label="创建时间" :labelCol="labelCol" :wrapperCol="wrapperCol">
              <a-range-picker
                style="width: 210px"
                v-model="queryParam.createTimeRange"
                format="YYYY-MM-DD HH:MM:SS"
                :placeholder="['开始时间', '结束时间']"
                @change="onDateChange"
                @ok="onDateOk"
              />
            </a-form-item>
          </a-col>
          <a-col :md="6" :sm="8">
            <span style="float: left;overflow: hidden;" class="table-page-search-submitButtons">
              <a-button type="primary" @click="searchQuery" icon="search">筛选</a-button>
              <a-button type="primary" @click="searchReset" icon="reload" style="margin-left: 8px">重置</a-button>
            </span>
          </a-col>


          <a-col :md="6" :sm="12">
            <a-form-item label="搜索">
              <j-input placeholder="请输入物料号查询" v-model="queryParam.materialCode"></j-input>
            </a-form-item>
          </a-col>
          <a-col :md="6" :sm="8">
            <span style="float: left;overflow: hidden;" class="table-page-search-submitButtons">
              <a-button type="primary" @click="searchQuery" icon="search">搜索</a-button>
              <a-button type="primary" @click="searchReset" icon="reload" style="margin-left: 8px">重置</a-button>
<!--              <a @click="handleToggleSearch" style="margin-left: 8px">
                {{ toggleSearchStatus ? '收起' : '展开' }}
                <a-icon :type="toggleSearchStatus ? 'up' : 'down'"/>
              </a>-->
            </span>
          </a-col>
        </a-row>
      </a-form>
    </div>
    <!-- 查询区域-END -->

    <!-- 操作按钮区域 -->
    <div class="table-operator">
      <a-button @click="handleAdd" type="primary" icon="plus">送检申请</a-button>

      <!-- 高级查询区域 -->
<!--      <j-super-query :fieldList="superFieldList" ref="superQueryModal" @handleSuperQuery="handleSuperQuery"></j-super-query>-->
      <a-dropdown v-if="selectedRowKeys.length > 0">
        <a-menu slot="overlay">
          <a-menu-item key="1" @click="batchDel"><a-icon type="delete"/>删除</a-menu-item>
        </a-menu>
        <a-button style="margin-left: 8px"> 批量操作 <a-icon type="down" /></a-button>
      </a-dropdown>
    </div>

    <!-- table区域-begin -->
    <div>
      <div class="ant-alert ant-alert-info" style="margin-bottom: 16px;">
        <i class="anticon anticon-info-circle ant-alert-icon"></i> 已选择 <a style="font-weight: 600">{{ selectedRowKeys.length }}</a>项
        <a style="margin-left: 24px" @click="onClearSelected">清空</a>
      </div>

      <a-table
        ref="table"
        size="middle"
        :scroll="{x:true}"
        bordered
        rowKey="id"
        :columns="columns"
        :dataSource="dataSource"
        :pagination="ipagination"
        :loading="loading"
        :rowSelection="{selectedRowKeys: selectedRowKeys, onChange: onSelectChange}"
        class="j-table-force-nowrap"
        @change="handleTableChange">

        <template slot="htmlSlot" slot-scope="text">
          <div v-html="text"></div>
        </template>
        <template slot="imgSlot" slot-scope="text">
          <span v-if="!text" style="font-size: 12px;font-style: italic;">无图片</span>
          <img v-else :src="getImgView(text)" height="25px" alt="" style="max-width:80px;font-size: 12px;font-style: italic;"/>
        </template>
        <template slot="fileSlot" slot-scope="text">
          <span v-if="!text" style="font-size: 12px;font-style: italic;">无文件</span>
          <a-button
            v-else
            :ghost="true"
            type="primary"
            icon="download"
            size="small"
            @click="downloadFile(text)">
            下载
          </a-button>
        </template>

        <span slot="action" slot-scope="text, record">
          <a @click="handleEdit(record)">编辑</a>

          <a-divider type="vertical" />
          <a-dropdown>
            <a class="ant-dropdown-link">更多 <a-icon type="down" /></a>
            <a-menu slot="overlay">
              <a-menu-item>
                <a @click="handleDetail(record)">详情</a>
              </a-menu-item>
              <a-menu-item>
                <a-popconfirm title="确定删除吗?" @confirm="() => handleDelete(record.id)">
                  <a>删除</a>
                </a-popconfirm>
              </a-menu-item>
            </a-menu>
          </a-dropdown>
        </span>

      </a-table>
    </div>

    <qzqm-check-info-modal ref="modalForm" @ok="modalFormOk"></qzqm-check-info-modal>
  </a-card>
</template>

<script>

  import '@/assets/less/TableExpand.less'
  import { mixinDevice } from '@/utils/mixin'
  import { JeecgListMixin } from '@/mixins/JeecgListMixin'
  import QzqmCheckInfoModal from './modules/QzqmCheckInfoModal'

  export default {
    name: 'QzqmCheckInfoList',
    mixins:[JeecgListMixin, mixinDevice],
    components: {
      QzqmCheckInfoModal
    },
    data () {
      return {
        description: 'qzqm_check_info管理页面',
        // 表头
        columns: [
          {
            title: '#',
            dataIndex: '',
            key:'rowIndex',
            width:60,
            align:"center",
            customRender:function (t,r,index) {
              return parseInt(index)+1;
            }
          },
          {
            title:'流程卡号',
            align:"center",
            dataIndex: 'workCode'
          },
          {
            title:'检测编号',
            align:"center",
            dataIndex: 'checkNo'
          },
          {
            title:'物料号',
            align:"center",
            dataIndex: 'materialCode'
          },
          {
            title:'物料名称',
            align:"center",
            dataIndex: 'materialName'
          },
          {
            title:'物料规格',
            align:"center",
            dataIndex: 'specifications'
          },
          {
            title:'图号',
            align:"center",
            dataIndex: 'productDraw'
          },
          {
            title:'客户名称',
            align:"center",
            dataIndex: 'customerName'
          },
          {
            title:'要求硬度',
            align:"center",
            dataIndex: 'hardness'
          },
          {
            title:'当前工序',
            align:"center",
            dataIndex: 'process'
          },
          {
            title:'材质',
            align:"center",
            dataIndex: 'material'
          },
          {
            title:'送检时间',
            align:"center",
            dataIndex: 'deliveryTime',
            customRender:function (text) {
              return !text?"":(text.length>10?text.substr(0,10):text)
            }
          },
          {
            title:'送检人',
            align:"center",
            dataIndex: 'deliveryUserId'
          },
          {
            title:'送检人姓名',
            align:"center",
            dataIndex: 'deliveryUserName'
          },
          {
            title:'送检部门',
            align:"center",
            dataIndex: 'deliveryDep'
          },
          {
            title:'检验员id',
            align:"center",
            dataIndex: 'checkUserId'
          },
          {
            title:'检验员名称',
            align:"center",
            dataIndex: 'checkUserName'
          },
          {
            title:'检测流程',
            align:"center",
            dataIndex: 'checkProcedure'
          },
          {
            title:'检验设备',
            align:"center",
            dataIndex: 'checkDevice'
          },
          {
            title:'检测时间',
            align:"center",
            dataIndex: 'checkTime',
            customRender:function (text) {
              return !text?"":(text.length>10?text.substr(0,10):text)
            }
          },
          {
            title:'预估时间',
            align:"center",
            dataIndex: 'evaluateTime'
          },
          {
            title:'上传报告地址',
            align:"center",
            dataIndex: 'reportUrl'
          },
          {
            title:'结束日期',
            align:"center",
            dataIndex: 'finishedTime',
            customRender:function (text) {
              return !text?"":(text.length>10?text.substr(0,10):text)
            }
          },
          {
            title:'检测状态',
            align:"center",
            dataIndex: 'checkStatus'
          },
          {
            title:'合格状态(0:未知，1:合格，2:不合格)',
            align:"center",
            dataIndex: 'qualifiedStatus'
          },
          {
            title:'是否已删除',
            align:"center",
            dataIndex: 'isDeleted'
          },
          {
            title: '操作',
            dataIndex: 'action',
            align:"center",
            fixed:"right",
            width:147,
            scopedSlots: { customRender: 'action' }
          }
        ],
        url: {
          list: "/check/qzqmCheckInfo/list",
          delete: "/check/qzqmCheckInfo/delete",
          deleteBatch: "/check/qzqmCheckInfo/deleteBatch",
          exportXlsUrl: "/check/qzqmCheckInfo/exportXls",
          importExcelUrl: "check/qzqmCheckInfo/importExcel",
          
        },
        dictOptions:{},
        superFieldList:[],
      }
    },
    created() {
    this.getSuperFieldList();
    },
    computed: {
      importExcelUrl: function(){
        return `${window._CONFIG['domianURL']}/${this.url.importExcelUrl}`;
      },
    },
    methods: {
      initDictConfig(){
      },
      getSuperFieldList(){
        let fieldList=[];
        fieldList.push({type:'string',value:'workCode',text:'流程卡号'})
        fieldList.push({type:'int',value:'checkNo',text:'检测编号'})
        fieldList.push({type:'string',value:'materialCode',text:'物料号'})
        fieldList.push({type:'string',value:'materialName',text:'物料名称'})
        fieldList.push({type:'string',value:'specifications',text:'物料规格'})
        fieldList.push({type:'string',value:'productDraw',text:'图号'})
        fieldList.push({type:'string',value:'customerName',text:'客户名称'})
        fieldList.push({type:'string',value:'hardness',text:'要求硬度'})
        fieldList.push({type:'string',value:'process',text:'当前工序'})
        fieldList.push({type:'string',value:'material',text:'材质'})
        fieldList.push({type:'date',value:'deliveryTime',text:'送检时间'})
        fieldList.push({type:'string',value:'deliveryUserId',text:'送检人'})
        fieldList.push({type:'string',value:'deliveryUserName',text:'送检人姓名'})
        fieldList.push({type:'string',value:'deliveryDep',text:'送检部门'})
        fieldList.push({type:'int',value:'checkUserId',text:'检验员id'})
        fieldList.push({type:'string',value:'checkUserName',text:'检验员名称'})
        fieldList.push({type:'int',value:'checkProcedure',text:'检测流程'})
        fieldList.push({type:'int',value:'checkDevice',text:'检验设备'})
        fieldList.push({type:'date',value:'checkTime',text:'检测时间'})
        fieldList.push({type:'int',value:'evaluateTime',text:'预估完成所需要的时间'})
        fieldList.push({type:'string',value:'reportUrl',text:'上传报告地址'})
        fieldList.push({type:'date',value:'finishedTime',text:'结束日期'})
        fieldList.push({type:'int',value:'checkStatus',text:'检测状态(0:待检测，1:检测中，2:检测完成)'})
        fieldList.push({type:'int',value:'qualifiedStatus',text:'合格状态(0:未知，1:合格，2:不合格)'})
        fieldList.push({type:'string',value:'isDeleted',text:'是否已删除'})
        this.superFieldList = fieldList
      }
    }
  }
</script>
<style scoped>
  @import '~@assets/less/common.less';
</style>
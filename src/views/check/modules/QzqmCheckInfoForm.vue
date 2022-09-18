<template>
  <a-spin :spinning="confirmLoading">
    <j-form-container :disabled="formDisabled">
      <a-form-model ref="form" :model="model" :rules="validatorRules" slot="detail">
        <a-divider>送检信息</a-divider>
        <a-row>
          <a-col :span="8">
            <a-form-model-item label="流程卡号" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="workCode">
              <a-input v-model="model.workCode" placeholder="请输入流程卡号"  @change="handleWorkCodeChange"></a-input>
            </a-form-model-item>
          </a-col>
<!--          <a-col :span="12">
            <a-form-model-item label="检测编号" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="checkNo">
              <a-input-number v-model="model.checkNo" placeholder="请输入检测编号" style="width: 100%" />
            </a-form-model-item>
          </a-col>-->
          <a-col :span="8">
            <a-form-model-item label="物料号" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="materialCode">
              <a-input v-model="model.materialCode" placeholder="请输入物料号"  ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="8">
            <a-form-model-item label="物料名称" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="materialName">
              <a-input v-model:value="model.materialName" placeholder="请输入物料名称"  ></a-input>
            </a-form-model-item>
          </a-col>
        </a-row>
        <a-row>
          <a-col :span="8">
            <a-form-model-item label="物料规格" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="specifications">
              <a-input v-model="model.specifications" placeholder="请输入物料规格"  ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="8">
            <a-form-model-item label="图号" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="productDraw">
              <a-input v-model="model.productDraw" placeholder="请输入图号"  ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="8">
            <a-form-model-item label="客户名称" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="customerName">
              <a-input v-model="model.customerName" placeholder="请输入客户名称"  ></a-input>
            </a-form-model-item>
          </a-col>
        </a-row>
        <a-row>
          <a-col :span="8">
            <a-form-model-item label="要求硬度" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="hardness">
              <a-input v-model="model.hardness" placeholder="请输入要求硬度"  ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="8">
            <a-form-model-item label="当前工序" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="process">
              <a-input v-model="model.process" placeholder="请输入当前工序"  ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="8">
            <a-form-model-item label="材质" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="material">
              <a-input v-model="model.material" placeholder="请输入材质"  ></a-input>
            </a-form-model-item>
          </a-col>
        </a-row>
        <a-row>
          <a-col :span="8">
            <a-form-model-item label="送检时间" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="deliveryTime">
              <j-date placeholder="请选择送检时间" v-model="model.deliveryTime"  style="width: 100%" />
            </a-form-model-item>
          </a-col>
          <a-col :span="8">
            <a-form-model-item label="送检人" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="deliveryUserId">
              <a-input v-model="model.deliveryUserId" placeholder="请输入送检人"  ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="8">
            <a-form-model-item label="送检人姓名" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="deliveryUserName">
              <a-input v-model="model.deliveryUserName" placeholder="请输入送检人姓名"  ></a-input>
            </a-form-model-item>
          </a-col>
        </a-row>
        <a-row>
          <a-col :span="8">
            <a-form-model-item label="送检部门" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="deliveryDep">
              <a-input v-model="model.deliveryDep" placeholder="请输入送检部门"  ></a-input>
            </a-form-model-item>
          </a-col>
        </a-row>
        <a-divider>检验信息</a-divider>
        <a-row>
          <a-col :span="8">
            <a-form-model-item label="检验员id" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="checkUserId">
              <a-input-number v-model="model.checkUserId" placeholder="请输入检验员id" style="width: 100%" />
            </a-form-model-item>
          </a-col>
          <a-col :span="8">
            <a-form-model-item label="检验员名称" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="checkUserName">
              <a-input v-model="model.checkUserName" placeholder="请输入检验员名称"  ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="8">
            <a-form-model-item label="检验设备" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="checkDevice">
              <a-input-number v-model="model.checkDevice" placeholder="请输入检验设备" style="width: 100%" />
            </a-form-model-item>
          </a-col>
        </a-row>
        <a-row>
          <a-col :span="8">
            <a-form-model-item label="检测时间" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="checkTime">
              <j-date placeholder="请选择检测时间" v-model="model.checkTime"  style="width: 100%" />
            </a-form-model-item>
          </a-col>
          <a-col :span="8">
            <a-form-model-item label="预估时间" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="evaluateTime">
              <a-input-number v-model="model.evaluateTime" placeholder="请输入预估完成所需要的时间" style="width: 100%" />
            </a-form-model-item>
          </a-col>
        </a-row>
        <a-divider>检验结果</a-divider>
        <a-row>
          <a-col :span="8">
            <a-form-model-item label="合格状态" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="qualifiedStatus">
              <a-input-number v-model="model.qualifiedStatus" placeholder="请输入合格状态" style="width: 100%" />
            </a-form-model-item>
          </a-col>
          <a-col :span="8">
            <a-form-model-item label="上传报告地址" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="reportUrl">
              <a-input v-model="model.reportUrl" placeholder="请输入上传报告地址"  ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="8">
            <a-form-model-item label="结束日期" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="finishedTime">
              <j-date placeholder="请选择结束日期" v-model="model.finishedTime"  style="width: 100%" />
            </a-form-model-item>
          </a-col>
        </a-row>
      </a-form-model>
    </j-form-container>
  </a-spin>
</template>

<script>

  import { httpAction, getAction } from '@/api/manage'
  import { validateDuplicateValue } from '@/utils/util'
  import request from "request";
  import debounce from "lodash/debounce";

  export default {
    name: 'QzqmCheckInfoForm',
    components: {
    },
    props: {
      //表单禁用
      disabled: {
        type: Boolean,
        default: false,
        required: false
      }
    },
    data () {
      return {
        model:{
         },
        labelCol: {
          xs: { span: 24 },
          sm: { span: 5 },
        },
        wrapperCol: {
          xs: { span: 24 },
          sm: { span: 16 },
        },
        confirmLoading: false,
        validatorRules: {
           workCode: [
              { required: true, message: '请输入流程卡号!'},
           ],
           checkNo: [
              { required: true, message: '请输入检测编号!'},
           ],
           materialCode: [
              { required: true, message: '请输入物料号!'},
           ],
           materialName: [
              { required: true, message: '请输入物料名称!'},
           ],
           specifications: [
              { required: true, message: '请输入物料规格!'},
           ],
           productDraw: [
              { required: true, message: '请输入图号!'},
           ],
           customerName: [
              { required: true, message: '请输入客户名称!'},
           ],
           hardness: [
              { required: true, message: '请输入要求硬度!'},
           ],
           process: [
              { required: true, message: '请输入当前工序!'},
           ],
           material: [
              { required: true, message: '请输入材质!'},
           ],
           deliveryTime: [
              { required: true, message: '请输入送检时间!'},
           ],
           deliveryUserId: [
              { required: true, message: '请输入送检人!'},
           ],
           deliveryUserName: [
              { required: true, message: '请输入送检人姓名!'},
           ],
           deliveryDep: [
              { required: true, message: '请输入送检部门!'},
           ],
           checkUserId: [
              { required: true, message: '请输入检验员id!'},
           ],
           checkUserName: [
              { required: true, message: '请输入检验员名称!'},
           ],
           checkStatus: [
              { required: true, message: '请输入检测状态(0:待检测，1:检测中，2:检测完成)!'},
           ],
           qualifiedStatus: [
              { required: true, message: '请输入合格状态(0:未知，1:合格，2:不合格)!'},
           ],
        },
        url: {
          add: "/check/qzqmCheckInfo/add",
          edit: "/check/qzqmCheckInfo/edit",
          queryById: "/check/qzqmCheckInfo/queryById",
          queryByWorkCode: "/check/qzqmCheckInfo/queryByWorkCode"
        }
      }
    },
    computed: {
      formDisabled(){
        return this.disabled
      },
    },
    created () {
       //备份model原始值
      this.modelDefault = JSON.parse(JSON.stringify(this.model));
    },
    methods: {
      add () {
        this.edit(this.modelDefault);
      },
      edit (record) {
        this.model = Object.assign({}, record);
        this.visible = true;
      },
      submitForm () {
        const that = this;
        // 触发表单验证
        this.$refs.form.validate(valid => {
          if (valid) {
            that.confirmLoading = true;
            let httpurl = '';
            let method = '';
            if(!this.model.id){
              httpurl+=this.url.add;
              method = 'post';
            }else{
              httpurl+=this.url.edit;
               method = 'put';
            }
            httpAction(httpurl,this.model,method).then((res)=>{
              if(res.success){
                that.$message.success(res.message);
                that.$emit('ok');
              }else{
                that.$message.warning(res.message);
              }
            }).finally(() => {
              that.confirmLoading = false;
            })
          }
         
        })
      },
      handleWorkCodeChange: debounce(async function (event){
        const workCode = event.target.value;
        const response = await getAction(this.url.queryByWorkCode, {workCode});
        this.model.materialCode = response.result.materialCode;
        this.model.materialName = response.result.materialName;
        this.model.specifications = response.result.specifications;
        this.model.productDraw = response.result.productDraw;
        this.model.customerName = response.result.customerName;
        this.model.hardness = response.result.hardness;
        this.model.process = response.result.process;
        this.model.material = response.result.material;
        this.model.deliveryTime = response.result.deliveryTime;
        this.model.deliveryUserId = response.result.deliveryUserId;
        this.model.deliveryUserName = response.result.deliveryUserName;
        this.model.deliveryDep = response.result.deliveryDep;

        this.model.checkUserId = response.result.checkUserId;
        this.model.checkUserName = response.result.checkUserName;
        this.model.deliveryDep = response.result.deliveryDep;
        this.model.deliveryDep = response.result.deliveryDep;
        this.model.deliveryDep = response.result.deliveryDep;

        this.$forceUpdate();
      },800),
    }
  }
</script>
<template>
  <a-spin :spinning="confirmLoading">
    <j-form-container :disabled="formDisabled">
      <a-form-model ref="form" :model="model" :rules="validatorRules" slot="detail">
        <a-divider>送检信息</a-divider>
        <a-row>
          <a-col :span="8">
            <a-form-model-item label="流程卡号" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="workCode">
              <a-input v-model="model.workCode" placeholder="请输入流程卡号"  @change="handleWorkCodeChange" :disabled="model.id"></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="8">
            <a-form-model-item label="物料号" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="materialCode">
              <a-input v-model="model.materialCode" placeholder="请输入物料号"  :disabled="model.id"></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="8">
            <a-form-model-item label="物料名称" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="materialName">
              <a-input v-model="model.materialName" placeholder="请输入物料名称"  :disabled="model.id"></a-input>
            </a-form-model-item>
          </a-col>
        </a-row>
        <a-row>
          <a-col :span="8">
            <a-form-model-item label="物料规格" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="specifications">
              <a-input v-model="model.specifications" placeholder="请输入物料规格"  :disabled="model.id"></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="8">
            <a-form-model-item label="图号" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="productDraw">
              <a-input v-model="model.productDraw" placeholder="请输入图号"  :disabled="model.id"></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="8">
            <a-form-model-item label="要求硬度" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="hardness">
              <a-input v-model="model.hardness" placeholder="请输入要求硬度"  :disabled="model.id"></a-input>
            </a-form-model-item>
          </a-col>
        </a-row>
        <a-row>
          <a-col :span="8">
            <a-form-model-item label="当前工序" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="process">
              <a-input v-model="model.process" placeholder="请输入当前工序"  :disabled="model.id"></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="8">
            <a-form-model-item label="工序详情" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="processDetail">
              <a-input v-model="model.processDetail" placeholder="请输入工序详情"  :disabled="model.id" ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="8">
            <a-form-model-item label="材质" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="material">
              <a-input v-model="model.material" placeholder="请输入材质"  :disabled="model.id"></a-input>
            </a-form-model-item>
          </a-col>
        </a-row>
        <a-row>
          <a-col :span="8">
            <a-form-model-item label="送检时间" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="deliveryTime">
              <j-date placeholder="请选择送检时间"  v-model="model.deliveryTime" :show-time="true" date-format="YYYY-MM-DD HH:mm:ss" style="width: 100%" :disabled="model.id"/>
            </a-form-model-item>
          </a-col>
          <a-col :span="8">
            <a-form-model-item label="送检人" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="deliveryUserId">
              <a-input v-model="model.deliveryUserId" placeholder="请输入送检人"  :disabled="model.id"></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="8">
            <a-form-model-item label="送检人姓名" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="deliveryUserName">
              <a-input v-model="model.deliveryUserName" placeholder="请输入送检人姓名"  :disabled="model.id"></a-input>
            </a-form-model-item>
          </a-col>
        </a-row>
        <a-row>
          <a-col :span="8">
            <a-form-model-item label="送检部门" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="deliveryDep">
              <a-input v-model="model.deliveryDep" placeholder="请输入送检部门"  :disabled="model.id"></a-input>
            </a-form-model-item>
          </a-col>
        </a-row>
        <a-divider>检验信息</a-divider>
        <a-row>
          <a-col :span="8">
            <a-form-model-item label="检验员id" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="checkUserId">
              <a-input v-model="model.checkUserId" placeholder="请输入检验员id" @change="handleDeliveryUserIdChange" :disabled="model.id"/>
            </a-form-model-item>
          </a-col>
          <a-col :span="8">
            <a-form-model-item label="检验员名称" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="checkUserName">
              <a-input v-model="model.checkUserName" placeholder="请输入检验员名称"  :disabled="model.id"></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="8">
            <a-form-model-item label="检验设备" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="checkDevice">
              <j-dict-select-tag type="list" v-model="model.checkDevice" dictCode="checkDevice" placeholder="请选择检验设备" :disabled="model.id"/>
            </a-form-model-item>
          </a-col>
        </a-row>
        <a-row>
          <a-col :span="8">
            <a-form-model-item label="检测时间" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="checkTime">
              <j-date placeholder="请选择检测时间"  v-model="model.checkTime" :show-time="true" date-format="YYYY-MM-DD HH:mm:ss" style="width: 100%" :disabled="model.id"/>
            </a-form-model-item>
          </a-col>
          <a-col :span="8">
            <a-form-model-item label="预估时间" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="evaluateTime">
              <a-input-number v-model="model.evaluateTime" placeholder="请输入预估时间" style="width: 100%" :disabled="model.id"/>
            </a-form-model-item>
          </a-col>
        </a-row>
        <a-divider>检验结果</a-divider>
        <a-row>
          <a-col :span="8">
            <a-form-model-item label="合格状态" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="qualifiedStatus">
              <j-dict-select-tag type="list" v-model="model.qualifiedStatus" dictCode="qualifiedStatus" placeholder="请输入合格状态" />
            </a-form-model-item>
          </a-col>
          <a-col :span="8">
            <a-form-model-item label="报告上传" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="reportUrl">
              <j-image-upload class="avatar-uploader" text="上传" v-model="model.reportUrl" is-multiple></j-image-upload>
            </a-form-model-item>
          </a-col>
          <a-col :span="8">
            <a-form-model-item label="结束日期" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="finishedTime">
              <j-date placeholder="请选择结束日期" v-model="model.finishedTime"  :show-time="true" date-format="YYYY-MM-DD HH:mm:ss"   style="width: 100%" />
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
  import debounce from "lodash/debounce";

  export default {
    name: 'QzqmCheckingInfoForm',
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
          sm: { span: 6 },
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
          checkDevice: [
            { required: true, message: '请输入检验设备!'},
          ],
          checkTime: [
            { required: true, message: '请输入检测时间!'},
          ],
          evaluateTime: [
            { required: true, message: '请输入预估时间!'},
          ],
          qualifiedStatus: [
            { required: true, message: '请输入合格状态(0:未知，1:合格，2:不合格)!'},
          ],
          reportUrl:[
            { required: true, message: '请上传结果'},
          ],
          finishedTime:[
            { required: true, message: '请输入结束时间'}
          ],
        },
        url: {
          add: "/check/qzqmCheckInfo/add",
          edit: "/check/qzqmCheckInfo/checked",
          queryById: "/check/qzqmCheckInfo/queryById",
          queryByWorkCode: "/check/qzqmCheckInfo/queryByWorkCode",
          queryMesUser:"/check/qzqmCheckInfo/queryMesUser"
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


      handleDeliveryUserIdChange: debounce(async function (event){
        const userId = event.target.value;
        const response = await getAction(this.url.queryMesUser, {userId});
        this.model.checkUserName = response.result.userName;
        this.$forceUpdate();
      },800),
    }
  }
</script>
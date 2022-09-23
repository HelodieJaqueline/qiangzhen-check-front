<template>
  <a-spin :spinning="confirmLoading">
    <j-form-container :disabled="formDisabled">
      <a-form-model ref="form" :model="model" :rules="validatorRules" slot="detail">
      <div>
        <a-divider>送检信息</a-divider>
        <a-row>
          <a-col :span="8">
            <a-form-model-item label="流程卡号" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="workCode">
              <a-input v-model="model.workCode" placeholder="请输入流程卡号"  @change="handleWorkCodeChange"></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="8">
            <a-form-model-item label="物料号" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="materialCode">
              <a-input v-model="model.materialCode" placeholder="请输入物料号"  ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="8">
            <a-form-model-item label="物料名称" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="materialName">
              <a-input v-model="model.materialName" placeholder="请输入物料名称"  ></a-input>
            </a-form-model-item>
          </a-col>
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
<!--          <a-col :span="8">
            <a-form-model-item label="客户名称" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="customerName">
              <a-input v-model="model.customerName" placeholder="请输入客户名称"  ></a-input>
            </a-form-model-item>
          </a-col>-->
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
          <a-col :span="8">
            <a-form-model-item label="送检时间" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="deliveryTime">
              <j-date placeholder="请选择送检时间"  v-model="model.deliveryTime" :show-time="true" date-format="YYYY-MM-DD HH:mm:ss" style="width: 100%" />
            </a-form-model-item>
          </a-col>
          <a-col :span="8">
            <a-form-model-item label="送检人" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="deliveryUserId">
              <a-input v-model="model.deliveryUserId" placeholder="请输入送检人" @change="handleDeliveryUserIdChange" ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="8">
            <a-form-model-item label="送检人姓名" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="deliveryUserName">
              <a-input v-model="model.deliveryUserName" placeholder="请输入送检人姓名"  ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="8">
            <a-form-model-item label="送检部门" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="deliveryDep">
              <a-input v-model="model.deliveryDep" placeholder="请输入送检部门"  ></a-input>
            </a-form-model-item>
          </a-col>
        </a-row>
      </div>
      <div v-if="model.id">
        <a-divider>检验信息</a-divider>
        <a-row>
          <a-col :span="8">
            <a-form-model-item label="检验员id" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="checkUserId">
              <a-input v-model="model.checkUserId" placeholder="请输入检验员id" @change="handleDeliveryUserIdChange"/>
            </a-form-model-item>
          </a-col>
          <a-col :span="8">
            <a-form-model-item label="检验员名称" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="checkUserName">
              <a-input v-model="model.checkUserName" placeholder="请输入检验员名称"  ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="6">
            <a-form-model-item label="检验设备" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="checkDevice">
              <j-dict-select-tag type="list" v-model="model.checkDevice" dictCode="checkDevice" placeholder="请选择检验设备" />
            </a-form-model-item>
          </a-col>
        </a-row>
        <a-row>
          <a-col :span="8">
            <a-form-model-item label="检测时间" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="checkTime">
              <j-date placeholder="请选择检测时间"  v-model="model.checkTime" :show-time="true" date-format="YYYY-MM-DD HH:mm:ss" style="width: 100%" />
            </a-form-model-item>
          </a-col>
          <a-col :span="8">
            <a-form-model-item label="预估完成所需要的时间" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="evaluateTime">
              <a-input-number v-model="model.evaluateTime" placeholder="请输入预估完成所需要的时间" style="width: 100%" />
            </a-form-model-item>
          </a-col>
        </a-row>
      </div>
      </a-form-model>
    </j-form-container>
  </a-spin>
</template>

<script>

  import { httpAction, getAction } from '@/api/manage'
  import { validateDuplicateValue } from '@/utils/util'
  import debounce from "lodash/debounce";

  export default {
    name: 'QzqmUncheckedInfoForm',
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
/*           customerName: [
              { required: true, message: '请输入客户名称!'},
           ],*/
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
        },
        url: {
          add: "/check/qzqmCheckInfo/add",
          edit: "/check/qzqmCheckInfo/checking",
          queryById: "/check/qzqmCheckInfo/queryById",
          queryWorkState: "/check/qzqmCheckInfo/queryWorkState",
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
        const response = await getAction(this.url.queryWorkState, {workCode});
        this.model.materialName = response.result.materialName;
        this.model.materialCode = response.result.materialCode;
        this.model.specifications = response.result.desc1;
        this.model.productDraw = response.result.productDraw;
        this.model.customerName = response.result.receiptName;
        this.model.hardness = response.result.desc2;
        this.model.process = response.result.pstationName;
        this.model.material = response.result.desc3;
        this.$forceUpdate();
      },800),

      handleDeliveryUserIdChange: debounce(async function (event){
        const userId = event.target.value;
        const response = await getAction(this.url.queryMesUser, {userId});
        this.model.deliveryUserName = response.result.userName;
        this.model.deliveryDep = response.result.dptName;
        this.$forceUpdate();
      },800),
    }
  }
</script>
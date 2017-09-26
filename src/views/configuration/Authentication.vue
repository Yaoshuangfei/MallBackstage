<template>
  <section>
    <el-col :span="24" style="position: relative;background: #cab78c;height:48px;line-height: 48px;color: #fff;font-size: 16px;padding-left: 20px;margin-bottom: 20px;">
      身份认证
    </el-col>
      <el-form id="sfrz" :model="sfinfo" label-width="135px"  :rules="rules" ref="sfinfo">
        <el-form-item label="公司名称:" prop="companyName">
          <el-input v-model="sfinfo.companyName" style="width:200px;"></el-input>
        </el-form-item>
        <el-form-item label="法人姓名:">
          <el-input v-model="sfinfo.realName" style="width:200px;"></el-input>
        </el-form-item>
        <el-form-item label="法人身份证号:" prop="legalCardCode">
          <el-input v-model="sfinfo.legalCardCode" style="width:400px;"></el-input>
        </el-form-item>
        <el-form-item label="手机号:">
          <el-input v-model="sfinfo.storeMobile" style="width:200px;"></el-input>
        </el-form-item>
        <el-form-item label="银行名称:">
          <el-input v-model="sfinfo.bankName" style="width:300px;"></el-input>
        </el-form-item>
        <el-form-item label="对公账户账号:">
          <el-input v-model="sfinfo.bankCode" style="width:400px;"></el-input>
        </el-form-item>
        <el-form-item label="运营地址:">
          <el-input v-model="sfinfo.theAddress" style="width:500px;"></el-input>
        </el-form-item>
        <el-form-item label="法人身份证:" style="position: relative;height:234px;">
          <input type="file" style="position:absolute;opacity:0;width:70px;height:30px;margin-right:10px;z-index: 1;"  @change="upload(1)" id="fileInput" >
          <button type="button" class="el-button el-button--primary el-button--small" style="width:92px;height:30px;background:#9f3333;font-size: 16px;color: #fff;border:1px solid #9f3333; ">
            <span >点击上传</span>
          </button>
          <span style="font-size: 12px;color: #ababab;margin-left: 10px;">图片格式：jpg,jpeg,png,gif，推荐大小800*800.</span>
          <img src="../../assets/upload.png" alt="" style="position: absolute;bottom:-180px;left:0;z-index: 0;width:160px;height:160px;">
          <el-col :span="24" v-if="frImgurl !== '' " style="position: absolute;bottom:-180px;left:0;z-index: 0;width:160px;height:160px;" ><img style="width: 160px;height:160px;" :src="frImgurl"></el-col>
        </el-form-item>
        <el-form-item label="法人手持身份证:" style="position: relative;height:234px;">
          <input type="file" style="position:absolute;opacity:0;width:70px;height:30px;margin-right:10px;z-index: 1;"  @change="upload(2)" id="fileInput" >
          <button type="button" class="el-button el-button--primary el-button--small" style="width:92px;height:30px;background:#9f3333;font-size: 16px;color: #fff;border:1px solid #9f3333; ">
            <span >点击上传</span>
          </button>
          <span style="font-size: 12px;color: #ababab;margin-left: 10px;">图片格式：jpg,jpeg,png,gif，推荐大小800*800.</span>
          <img src="../../assets/upload.png" alt="" style="position: absolute;bottom:-180px;left:0;z-index: 0;width:160px;height:160px;">
          <el-col :span="24" v-if="frImgurlPointer !== '' " style="position: absolute;bottom:-180px;left:0;z-index: 0;width:160px;height:160px;" ><img style="width: 160px;height:160px;" :src="frImgurlPointer"></el-col>
        </el-form-item>
        <el-form-item label="营业执照:" style="position: relative;height:234px;">
          <input type="file" style="position:absolute;opacity:0;width:70px;height:30px;margin-right:10px;z-index: 1;"  @change="upload(3)" id="fileInput" >
          <button type="button" class="el-button el-button--primary el-button--small" style="width:92px;height:30px;background:#9f3333;font-size: 16px;color: #fff;border:1px solid #9f3333; ">
            <span >点击上传</span>
          </button>
          <span style="font-size: 12px;color: #ababab;margin-left: 10px;">图片格式：jpg,jpeg,png,gif，推荐大小800*800.</span>
          <img src="../../assets/upload.png" alt="" style="position: absolute;bottom:-180px;left:0;z-index: 0;width:160px;height:160px;">
          <el-col :span="24" v-if="businessImgurl !== '' " style="position: absolute;bottom:-180px;left:0;z-index: 0;width:160px;height:160px;" ><img style="width: 160px;height:160px;" :src="businessImgurl"></el-col>
        </el-form-item>
        <el-form-item label="银行开户许可证:" style="position: relative;height:234px;">
          <input type="file" style="position:absolute;opacity:0;width:70px;height:30px;margin-right:10px;z-index: 1;"  @change="upload(4)" id="fileInput" >
          <button type="button" class="el-button el-button--primary el-button--small" style="width:92px;height:30px;background:#9f3333;font-size: 16px;color: #fff;border:1px solid #9f3333; ">
            <span >点击上传</span>
          </button>
          <span style="font-size: 12px;color: #ababab;margin-left: 10px;">图片格式：jpg,jpeg,png,gif，推荐大小800*800.</span>
          <img src="../../assets/upload.png" alt="" style="position: absolute;bottom:-180px;left:0;z-index: 0;width:160px;height:160px;">
          <el-col :span="24" v-if="bankImgurl !== '' " style="position: absolute;bottom:-180px;left:0;z-index: 0;width:160px;height:160px;" ><img style="width: 160px;height:160px;" :src="bankImgurl"></el-col>
        </el-form-item>

        <!--<el-col :span="24"  v-if="frImgurlPointer !== '' "><img style="width: 100px" :src="frImgurlPointer"></el-col>-->
        <!--<el-form-item label="法人手持身份证:">-->
          <!--<input type="file" style="position:absolute;opacity:0;width:70px;height:30px;margin-right:10px"  @change="upload(2)" id="fileInput">-->
          <!--<button type="button" class="el-button el-button&#45;&#45;primary el-button&#45;&#45;small">-->
            <!--<span>点击上传</span>-->
          <!--</button>-->
        <!--</el-form-item>-->
        <!--<el-col :span="24"  v-if="businessImgurl !== '' "><img style="width: 100px" :src="businessImgurl"></el-col>-->
        <!--<el-form-item label="营业执照:">-->
          <!--<input type="file" style="position:absolute;opacity:0;width:70px;height:30px;margin-right:10px"  @change="upload(3)" id="fileInput">-->
          <!--<button type="button" class="el-button el-button&#45;&#45;primary el-button&#45;&#45;small">-->
            <!--<span>点击上传</span>-->
          <!--</button>-->
        <!--</el-form-item>-->
        <!--<el-col :span="24" v-if="bankImgurl !== '' "><img style="width: 100px" :src="bankImgurl"></el-col>-->
        <!--<el-form-item label="银行开户许可证:">-->
          <!--<input type="file" style="position:absolute;opacity:0;width:70px;height:30px;margin-right:10px"  @change="upload(4)" id="fileInput">-->
          <!--<button type="button" class="el-button el-button&#45;&#45;primary el-button&#45;&#45;small">-->
            <!--<span>点击上传</span>-->
          <!--</button>-->
        <!--</el-form-item>-->
      </el-form>
      <div slot="footer" class="dialog-footer" style="text-align: center;">
        <el-button type="primary" @click.native="clupLoad">保存</el-button>
      </div>
  </section>
</template>

<script>
    import echarts from 'echarts'
    import { state } from '../../vuex/state'
    import {baseUrl} from '../../api/api';
    export default {
        data() {
            return {
                baobiaoName:'日营业额',
                addTitle:'添加身份信息',
                frImgurl:'',
                frImgurlPointer:'',
                businessImgurl:'',
                bankImgurl:'',
                formData: new FormData(),
                rules:{
                    legalCardCode: [
                        { required: true, message: '请输入身份证号', trigger: 'blur' },
                        { pattern: /(^[1-9]\d{5}(18|19|([23]\d))\d{2}((0[1-9])|(10|11|12))(([0-2][1-9])|10|20|30|31)\d{3}[0-9Xx]$)|(^[1-9]\d{5}\d{2}((0[1-9])|(10|11|12))(([0-2][1-9])|10|20|30|31)\d{2}$)/, message: '证件号码格式有误！', trigger: 'blur' }
                    ],
                    companyName:[{ required: true, message: '请输入公司名称', trigger: 'blur' },
            { min: 3, max: 20, message: '长度在 3 到 100 个字符', trigger: 'blur' }]
                },
                sfinfo:{
                    realName:'',
                    legalCardCode:'',
                    storeMobile:'',
                    bankName:'',
                    bankCode:'',
                    theAddress:'',
                    companyName:''
                },
                total:7,
                page: 1,
                totals:10,
                pages: 1,
                editFormVisible:false,//上传材料
                dashVisible:false,//待审核
                remarkInfo:'',//未通过信息
                sfinfoId:'',//修改身份信息ID
                daibanList:[],
                dingdanList:[],
                radio: 0,
                ruleAll:[{
                    name:'日报表',
                    id:0
                },{
                    name:'周报表',
                    id:1
                },{
                    name:'月报表',
                    id:2
                },{
                    name:'季报表',
                    id:3
                },{
                    name:'年报表',
                    id:4
                }],
                chartColumn: null,
                chartBar: null,
                chartLine: null,
                chartPie: null,
                chartmap: null,
                visitPercentage:'',
                visitCount:'',
                sumTotalMoney:'',
                moneyPercentage:'',
                memberCount:'',
                memberPercentage:'',
                sumTotalQuantity:'',
                quantityPercentage:'',
                type:0,

                listAll:[],//线图
                sj:[],
                cj:[],
                sp:[],
                pj:[],
                parobj:[],//饼图
                parName:[]
            }
        },
        methods: {
            //图片上传
            upload (index) {
                this.formData = new FormData()
                let file = event.target.files[0]
                const self = this
                if (file) {
                    this.fileImg = file.name
                    this.formData.append('file', file);
                    this.submitUpload(index)
                    console.log(file);
                } else {
                    this.fileImg = ''
                    this.formData = new FormData()
                }
            },
            submitUpload(index){
                const _this= this;
                _this.$http.post(baseUrl+'/api/attachment/upload', _this.formData, {
                    progress(event) {
                    }
                })
                    .then(response => {
                        const info = JSON.parse(response.bodyText);
                        if(index === 1){
                            _this.frImgurl = info.data[0].baseUri+info.data[0].uri;
                        }else if(index === 2){
                            _this.frImgurlPointer = info.data[0].baseUri+info.data[0].uri;
                        }else if(index === 3){
                            _this.businessImgurl = info.data[0].baseUri+info.data[0].uri;
                        }else if(index === 4){
                            _this.bankImgurl = info.data[0].baseUri+info.data[0].uri;
                        }
                    }, error => _this.$emit('complete', 500, error.message))
            },
            clupLoad(){
                const _this = this
                const params = {
                    realName:this.sfinfo.realName,
                    legalCardCode:this.sfinfo.legalCardCode,
                    storeMobile:this.sfinfo.storeMobile,
                    bankName:this.sfinfo.bankName,
                    bankCode:this.sfinfo.bankCode,
                    theAddress:this.sfinfo.theAddress,
                    cardImgF:this.frImgurl,
                    cardImgW:this.frImgurlPointer,
                    businessLicense:this.businessImgurl,
                    bankImgW:this.bankImgurl,
                    companyName:this.sfingo.companyName
                }
                let url = ''
                console.log(this.sfinfoId)
                if(this.sfinfoId === ''){
                    url = '/api/coreUspAuthentication/add'
                }else{
                    url = '/api/coreUspAuthentication/updateDetails'
                    params.id = this.sfinfoId
                }
                console.log(url)
                $.ajax({
                    type:'POST',
                    dataType:'json',
                    url:baseUrl+url,
                    data:JSON.stringify(params),
                    contentType:'application/json;charset=utf-8',
                    success:function(data){
                        if(data.code === 1){
                            _this.$message({
                                message: data.msg,
                                type: 'success'
                            });
                        }else{
                            _this.$message.error(data.msg);
                        }
                    }
                });
            },
            getSHinfo(){
                const _this = this
                $.ajax({
                  type:'POST',
                  dataType:'json',
                  url:baseUrl+"/api/coreUspAuthentication/checkDetails",
                  data:JSON.stringify({}),
                  contentType:'application/json;charset=utf-8',
                  success:function(data){
                    console.log(data)
                    const info = data.data
                    if(info === null){

                    }else{
                        _this.remarkInfo = info.remark
                        _this.sfinfoId = info.id
                        _this.sfinfo.companyName = info.companyName
                        _this.sfinfo.realName = info.realName
                        _this.sfinfo.legalCardCode = info.legalCardCode
                        _this.sfinfo.storeMobile = info.storeMobile
                        _this.sfinfo.bankName = info.bankName
                        _this.sfinfo.bankCode = info.bankCode
                        _this.sfinfo.theAddress  = info.theAddress
                        _this.frImgurl   = info.cardImgF
                        _this.frImgurlPointer  = info.cardImgW
                        _this.businessImgurl   = info.businessLicense
                        _this.bankImgurl = info.bankImgW
                        console.log(_this.sfinfo)
                    }
                  }
              });
            },
        },
        mounted: function () {
            this.getSHinfo()
        },
        // updated: function () {
        //     this.drawCharts()
        // }
    }
</script>

<style>
  #sfrz .el-form-item__label {margin-right: inherit;text-align: right;font-size: 16px;color: #616161;}
</style>
<template>
	<section>
        <el-col :span="24" v-show="oneShow">
            <el-col :span="24" style="margin-bottom: 20px;font-size: 18px;">基本信息</el-col>
            <el-col :span="24" class="booton_margin">
               <el-col :span="2" style="margin-top: 6px;">活动名称：</el-col>
               <el-col :span="10">
                    <el-input v-model="name"></el-input>
                    <!-- <el-col :span="24" style="margin-bottom: 20px;">
                        <el-col :span="6">
                            <el-radio class="radio" v-model="activityName" label="1">日常活动</el-radio>
                        </el-col>
                        <el-col :span="14" style="margin-top: -10px;">
                            <el-input v-model="name"></el-input>
                        </el-col>
                    </el-col>
                    <el-col :span="24">
                        <el-radio class="radio" v-model="activityName" label="2">官方活动</el-radio>
                    </el-col> -->
               </el-col>
            </el-col>
            <el-col :span="24" class="booton_margin">
               <el-col :span="2" style="margin-top: 6px;">活动图：</el-col>
               <el-col :span="10">
                   <input type="file" style="position:absolute;opacity:0;width:70px;height:30px;margin-right:10px"  @change="upload" id="fileInput">
                    <button type="button" class="el-button el-button--primary el-button--small">
                        <span>点击上传</span>
                    </button>
               </el-col>
            </el-col>
            <el-col :span="24" style="margin-left:120px;margin-bottom: 20px;">
                <img v-if="activityLogo !== ''" style="width: 150px;" :src="activityLogo">
            </el-col>
            <el-col :span="24" class="booton_margin">
               <el-col :span="2" style="margin-top: 6px;">活动描述：</el-col>
               <el-col :span="10">
                   <el-input v-model="activityDetails"></el-input>
               </el-col>
            </el-col>	
            <el-col :span="24" class="booton_margin">
               <el-col :span="2" style="margin-top: 6px;">开始时间：</el-col>
               <el-col :span="10">
                    <el-date-picker
                      v-model="begtime"
                      type="datetime"
                      placeholder="选择开始时间">
                    </el-date-picker>
               </el-col>
            </el-col>
            <el-col :span="24" class="booton_margin">
               <el-col :span="2" style="margin-top: 6px;">结束时间：</el-col>
               <el-col :span="10">
                   <el-date-picker
                      v-model="endtime"
                      type="datetime"
                      placeholder="选择结束时间">
                    </el-date-picker>
               </el-col>
            </el-col>
            <el-col :span="24" class="booton_margin">
               <el-col :span="2" style="margin-top: 6px;">是否专属分佣：</el-col>
               <el-col :span="10" class="radio_b">
                    <el-radio class="radio" v-model="isCommission" label="1">是</el-radio>
                    <el-radio class="radio" v-model="isCommission" label="0">否</el-radio>
               </el-col>
            </el-col>
            <el-col :span="24" class="booton_margin">
               <el-col :span="2" style="margin-top: 6px;">优惠方式：</el-col>
               <el-col :span="10" class="radio_b">
                   <el-radio class="radio" disabled  v-model="activityRulesType" label="1">满就送</el-radio>
                   <el-radio class="radio" disabled  v-model="activityRulesType" label="2">满就减</el-radio>
                   <el-radio class="radio" disabled  v-model="activityRulesType" label="3">限时打折</el-radio>
               </el-col>
            </el-col>
            <el-col :span="24" class="booton_margin">
               <el-col :span="3" style="margin-top: 6px;">优惠满足方式：</el-col>
               <el-col :span="10" class="radio_b">
                   <el-radio class="radio" disabled  v-model="valuationType" label="0">按数量</el-radio>
                   <el-radio class="radio" disabled  v-model="valuationType" label="1">按金额</el-radio>
               </el-col>
            </el-col>
            <el-col :span="24" class="booton_margin">
               <el-col :span="2" style="margin-top: 6px;">活动包邮：</el-col>
               <el-col :span="10" class="radio_b">
                   <el-radio class="radio" v-model="isMail" label="1">是</el-radio>
                   <el-radio class="radio" v-model="isMail" label="2">否</el-radio>
               </el-col>
            </el-col>	
            <el-col :span="24" style="margin-top: 20px;margin-left: 40px;">
               <el-col :span="4">
                    <el-button type="primary" @click="updata">下一步</el-button>
               </el-col>
               <el-col :span="4">
                    <el-button type="primary">取消</el-button>
               </el-col>
            </el-col>   
        </el-col>
        <el-col :span="24" v-show="twoShow">
            <el-col :span="24" style="margin-bottom: 20px;">
                 <el-button type="primary" @click="onGoods">添加商品</el-button>
            </el-col>
            <el-table :data="table" @selection-change="selsChange" border highlight-current-row v-loading="listLoading" style="width: 100%;min-width: 1080px;text-align: center;">
                <el-table-column type="selection">
                </el-table-column>
                <el-table-column type="carouselPicture" label="图片" width="150px;">
                    <template scope="scope">
                        <img style="width:100px;" :src="scope.row.carouselPicture[0]">
                    </template>
                </el-table-column>
                <el-table-column prop="name" label="商品名称">
                </el-table-column>
                <el-table-column prop="price" label="价格">
                </el-table-column>
                <el-table-column prop="kcNum" label="库存">
                </el-table-column>
                <!-- <el-table-column label="操作">
                    <template scope="scope">
                        <el-button type="text" size="small" v-if="scope.row.orderStatus !== 11" @click="shenheBtn(scope.row)">审核</el-button>
                        <el-button type="text" size="small" @click="upBtn(scope.row)">充值</el-button>
                        <el-button type="text" size="small" @click="notpassBtn(scope.row)">不通过</el-button>
                    </template>
                </el-table-column> -->
            </el-table>
        </el-col>
        <el-col :span="24" v-show="treeShow">
            <el-col :span="24" style="margin-bottom: 20px;">
                <el-col :span="23">
                    <el-button type="primary" @click="showGoods">添加商品</el-button>
                </el-col>
                <el-col :span="1">
                    <el-button type="primary" @click="upGoods">保存</el-button>
                </el-col>
            </el-col><!-- @selection-change="selsChange" -->
            <el-table :data="tabletest"  border highlight-current-row v-loading="listLoading" style="width: 100%;min-width: 1080px;text-align: center;">
                <!-- <el-table-column type="selection">
                </el-table-column> -->
                <!-- <el-table-column type="carouselPicture" label="图片" width="150px;">
                    <template scope="scope">
                        <img style="width:100px;" :src="scope.row.carouselPicture[0]">
                    </template>
                </el-table-column> -->
                <el-table-column prop="name" label="商品名称">
                </el-table-column>
                <el-table-column prop="price" label="价格">
                </el-table-column>
                <el-table-column prop="kcNum" label="库存">
                </el-table-column>
                <el-table-column :label="manfangs">
                    <template scope="scope">
                        <el-input v-model="scope.row.statusType" type="number"></el-input>
                    </template>
                </el-table-column>
                <el-table-column v-if="song" prop="value" label="赠送商品">
                    <template scope="scope">
                        {{scope.row.seller}}
                        <el-select v-model="scope.row.value" placeholder="请选择">
                            <el-option
                              v-for="item in options"
                              :label="item.label"
                              :value="item.value">
                            </el-option>
                        </el-select>
                    </template>
                </el-table-column>
                <el-table-column :label="numberMan">
                    <template scope="scope">
                        <el-input v-model="scope.row.manNub" type="number"></el-input>
                    </template>
                </el-table-column>
                <!-- <el-table-column label="限购数量" width="130px">
                    <template scope="scope">
                        <el-select v-model="scope.row.value" placeholder="请选择">
                            <el-option
                              v-for="item in options"
                              :key="item.value"
                              :label="item.label"
                              :value="item.value">
                            </el-option>
                        </el-select>
                    </template>
                </el-table-column> -->
                <el-table-column v-if="isCommissions" prop="fyNum" label="分佣配置">
                    <template scope="scope">
                        <el-button type="text" v-if="scope.row.show === '1' " size="small" @click="addgz(scope.row)">添加规则</el-button>
                        <el-button type="text" v-else size="small" @click="addgztwo(scope.row)">添加规则</el-button>
                    </template>
                </el-table-column>
                <!-- <el-table-column label="操作">
                    <template scope="scope">
                        <el-button type="text" size="small" v-if="scope.row.orderStatus !== 11" @click="shenheBtn(scope.row)">审核</el-button>
                        <el-button type="text" size="small" @click="upBtn(scope.row)">充值</el-button>
                        <el-button type="text" size="small" @click="notpassBtn(scope.row)">不通过</el-button>
                    </template>
                </el-table-column> -->
            </el-table>
        </el-col>
        <el-dialog title="分佣规则配置" v-model="goodList" :close-on-click-modal="false" >
            <el-col v-for="item in fyObjList" :span="18" :offset="8" style="margin-bottom: 20px;">
                <el-col :span="2" style="margin-top: 8px;">{{item.id}}级</el-col>
                <el-col :span="8">
                    <el-input v-model="item.value" type="number"></el-input>
                </el-col>
            </el-col>
            <el-col :span="24">
                <el-col :span="3" :offset="14"><el-button type="text" @click="addfyBtn">添加+</el-button></el-col>
            </el-col>
            <div slot="footer" class="dialog-footer" style="text-align: center;">
                <el-button type="primary" @click.native="goodList = false">确定</el-button>
                <!-- <el-button type="primary" @click.native="goodList = false">关闭</el-button> -->
            </div>
        </el-dialog>
	</section>
</template>

<script>
	import { state } from '../../vuex/state'
	import util from '../../common/js/util'
	import {baseUrl} from '../../api/api';

	export default {
		data() {
			return {
                fyObjList:[],
                tabletest:[],
                value:'0',
                initObj:[],
                goostable:[],
                id:'',
                goodList:false,
                song:false,
                oneShow:true,
                twoShow:false,
                treeShow:false,
                activityName:'1',
                name:'',//活动名称
                activityDetails:'',//描述
                begtime:'',//开始时间
                endtime:'',//结束时间
                activityRulesType:'1',//优惠方式
                valuationType:'',//优惠满足方式
                isMail:'',//是否包邮
                isCommission:'',//是否分佣
                activityLogo:'',//活动图
                formData: new FormData(),
                table:[],
                listLoading:false,
                treetable:[],
                manfangs:'打折',
                numberMan:'金额',
                isCommissions:true,
                options:[
                    {
                        value:'0',
                        label:'不限购'
                    },
                    {
                        value:'1',
                        label:'限购'
                    },
                ]
			}
		},
		methods: {
            // 获取活动详情
            getActivity(){
                const _this = this
                const params = {
                    activityId:this.$route.params.id
                }
                console.log(params)
                $.ajax({
                    type:'POST',
                    dataType:'json',
                    url:baseUrl+"/api/activity/selectOne",
                    data:JSON.stringify(params),
                    contentType:'application/json;charset=utf-8',
                    success:function(data){
                        console.log(data)
                        _this.name = data.data.activityName
                        _this.activityDetails = data.data.activityDetails
                        _this.begtime= data.data.beginTime//开始时间
                        _this.endtime= data.data.endTime//结束时间
                        _this.activityRulesType = data.data.activityRulesType.toString()//优惠方式
                        _this.valuationType = data.data.valuationType.toString()//优惠满足方式
                        _this.isMail = data.data.isMail.toString()
                        _this.isCommission = data.data.isCommission.toString()
                        _this.activityLogo = data.data.activityLogo
                        if(_this.activityRulesType === '1'){
                            _this.manfangs = '赠送数量'// 送商品 填写ID
                            _this.song = true
                        }else if(_this.activityRulesType === '2'){
                            _this.manfangs = '满就减'
                        }else if(_this.activityRulesType === '3'){
                            _this.manfangs = '限时打折'
                        }
                        // 0 按数量 1 金额 data.data.valuationType 
                        if(_this.valuationType === '0'){
                            _this.numberMan = '最低购买数量'
                        }else if(_this.valuationType === '1'){
                            _this.numberMan = '最低消费金额'
                        }
                        //data.data.isCommission 0 no 1 yes      isMail  1 yes 2 no  
                        if(_this.isCommission === '0'){
                            _this.isCommissions = false
                        }else if(_this.isCommission === '1'){
                            _this.isCommissions = true
                        }
                    }
                })
            },
            // 活动配置
			updata(){
                const _this= this
                if(this.begtime === '' || this.endtime === ''){
                    alert('请填写开始时间或结束时间')
                    return
                }
                console.log(_this.activityRulesType)
                const params = {
                    activityId:this.$route.params.id,
                    activityName:this.name,
                    activityLogo:this.activityLogo,
                    activityDetails:this.activityDetails,
                    valuationType:this.valuationType,
                    isMail:this.isMail,
                    activityRulesType:this.activityRulesType,
                    isCommission:this.isCommission
                }
                var re = /^[0-9]+.?[0-9]*$/
            　　if (!re.test(this.begtime)) {
                    params.getBeginTime = this.begtime.getTime()
            　　}else{
                    params.getBeginTime = this.begtime
                }
                if (!re.test(this.endtime)) {
                    params.getEndTime = this.endtime.getTime()
            　　}else{
                    params.getEndTime = this.endtime
                }
                console.log(params)
                $.ajax({
                    type:'POST',
                    dataType:'json',
                    url:baseUrl+"/api/activity/update",
                    data:JSON.stringify(params),
                    contentType:'application/json;charset=utf-8',
                    success:function(data){
                        console.log(data)
                        if(data.code === 1){
                            _this.oneShow = false
                            _this.treeShow = true
                            _this.id = data.data.activityId
                            _this.getGoods()
                            // 1 满就送 2 满就减 3 限时打折 data.data.activityRulesType
                            if(_this.activityRulesType === '1'){
                                _this.manfangs = '赠送数量'// 送商品 填写ID
                                _this.song = true
                            }else if(_this.activityRulesType === '2'){
                                _this.manfangs = '满就减'
                            }else if(_this.activityRulesType === '3'){
                                _this.manfangs = '限时打折'
                            }
                            // 0 按数量 1 金额 data.data.valuationType 
                            if(data.data.valuationType === 0){
                                _this.numberMan = '最低购买数量'
                            }else if(data.data.valuationType === 1){
                                _this.numberMan = '最低消费金额'
                            }
                            //data.data.isCommission 0 no 1 yes      isMail  1 yes 2 no  
                            if(data.data.isCommission === 0){
                                _this.isCommissions = false
                            }else if(data.data.isCommission === 1){
                                _this.isCommissions = true
                            }
                        }else{
                            _this.$message({
                                message: data.msg,
                                type: 'error'
                            });
                        }
                    }
                })
			},
            // 获取活动内的商品
            getGoods(){
                const _this = this
                const params = {
                    activityId:this.$route.params.id 
                }
                console.log(params)
                $.ajax({
                    type:'POST',
                    dataType:'json',
                    url:baseUrl+"/api/activity/selectListOfGoods",
                    data:JSON.stringify(params),
                    contentType:'application/json;charset=utf-8',
                    success:function(data){
                        console.log(data)
                        const list = data.data
                        for (var i = 0; i < list.length; i++) {
                            const obj = {}
                            obj.goodsId = list[i].goodsId
                            obj.fyNum = []
                            obj.comData = list[i].comData
                            obj.statusType = list[i].activityValue
                            obj.value = list[i].activityObj
                            obj.manNub = list[i].minNum
                            obj.maxNum = list[i].maxNum
                            obj.show = '1'
                            _this.initObj.push(obj)
                        }
                        console.log(_this.initObj)
                        _this.getList()
                    }
                })
            },
            getList(){
                const _this= this
                _this.options = []
                const params = {
                    storeId:localStorage.getItem("storeId"),
                    activityId:this.$route.params.id ,
                    name:''
                }
                console.log(params)
                $.ajax({
                    type:'POST',
                    dataType:'json',
                    url:baseUrl+"/api/activity/selectGoodsList",
                    data:JSON.stringify(params),
                    contentType:'application/json;charset=utf-8',
                    success:function(data){
                        const info = data.data
                        _this.table = info
                        for (var i = 0; i < _this.table.length; i++) {
                            const optionObj = {}
                            optionObj.value = _this.table[i].id.toString()
                            optionObj.label = _this.table[i].name
                            _this.options.push(optionObj)
                            _this.table[i].carouselPicture = _this.table[i].carouselPicture.split(',')
                            let num = 0
                            for (var x = 0; x < _this.table[i].goodsSpecs.length; x++) {
                                num +=  _this.table[i].goodsSpecs[x].storage
                                 _this.table[i].kcNum = num
                            }
                        }
                        for (var i = 0; i < _this.initObj.length; i++) {
                            for (var x = 0; x < _this.table.length; x++) {
                                if(_this.initObj[i].goodsId === _this.table[x].id){
                                    _this.initObj[i].name = _this.table[x].name
                                    _this.initObj[i].price = _this.table[x].price
                                    _this.initObj[i].kcNum = _this.table[x].kcNum
                                }
                            }
                        }
                        _this.tabletest = _this.initObj
                        
                    }
                })
            },
            // 批量选择
            selsChange: function (sels) {
                this.treetable = sels
            },
            onGoods(){
                for (var i = 0; i < this.treetable.length; i++) {
                    const obj = {}
                    obj.value = ''
                    obj.name = this.treetable[i].name
                    obj.goodsId = this.treetable[i].id
                    obj.price = this.treetable[i].price
                    obj.kcNum = this.treetable[i].kcNum
                    obj.statusType = '1'
                    obj.manNub = this.treetable[i].manNub
                    obj.fyNum = [{
                        id:1,
                        value:''
                    }]
                    this.tabletest.push(obj)
                }
                this.twoShow = false
                this.treeShow = true
            },
            upGoods(){
                console.log(this.tabletest)
                const _this = this
                const arry = []
                for (var i = 0; i < this.tabletest.length; i++) {
                    if(this.tabletest[i].show === 1){
                        this.tabletest[i].fyNum = eval('(' + this.tabletest[i].fyNum + ')')
                    }
                    const obj = {}
                    obj.goodsId = this.tabletest[i].goodsId
                    obj.comData = JSON.stringify(this.tabletest[i].fyNum)
                    obj.minNum = this.tabletest[i].manNub
                    obj.maxNum = '0'
                    obj.activityValue = this.tabletest[i].statusType
                    obj.activityObj = this.tabletest[i].value
                    arry.push(obj)
                }
                const params = {
                    activityId:this.$route.params.id ,
                    // activityId:30,
                    activityGoodsList:arry
                }
                console.log(params)
                $.ajax({
                    type:'POST',
                    dataType:'json',
                    url:baseUrl+"/api/activity/addActivityGoods",
                    data:JSON.stringify(params),
                    contentType:'application/json;charset=utf-8',
                    success:function(data){
                        console.log(data)
                        if(data.code === 1){ 
                            _this.$router.push({ path: '/activity' });
                        }else{
                            _this.$message({
                                message: data.msg,
                                type: 'error'
                            });
                        }
                    }
                })
            },
            addgz(row){
                console.log(row)
                this.goodList = true
                // this.fyObjList = row.fyNum
                if(row.comData){
                    const comData = eval('(' + row.comData + ')')
                    row.fyNum = comData
                    this.fyObjList =eval('(' + row.comData + ')')
                }else{
                    this.fyObjList = row.fyNum
                }
                row.show = '0'
                console.log(row)
            },
            addgztwo(row){
                this.goodList = true
                this.fyObjList = row.fyNum
            },
            addfyBtn(){
                const obj = {}
                obj.id = this.fyObjList.length +1
                obj.value = ''
                this.fyObjList.push(obj)
            },
            showGoods(){
                this.twoShow = true
                this.treeShow = false
            },






            //图片上传
            upload (event) {
                this.formData = new FormData()
                let file = event.target.files[0]
                const self = this
                if (file) {
                    this.fileImg = file.name
                    this.formData.append('file', file);
                    this.submitUpload()
                } else {
                    this.fileImg = ''
                    this.formData = new FormData()
                }
            },
            submitUpload(){
                const _this= this;
                _this.$http.post(baseUrl+'/api/attachment/upload', _this.formData, {
                    progress(event) {
                    }
                })
                    .then(response => {
                        const info = JSON.parse(response.bodyText);
                        _this.activityLogo = info.data[0].baseUri+info.data[0].uri;
                    }, error => _this.$emit('complete', 500, error.message))
            },
		},
		mounted() {
            this.getActivity()
            if(this.$route.params.index === 0){
                this.oneShow = false
                this.treeShow = true
                this.getGoods()
            }
		}
	}
</script>
<style type="text/css">
    .booton_margin{
        margin-bottom: 20px;
        margin-left: 30px;
    }
    .radio_b{
        margin-top: 6px;
    }
</style>
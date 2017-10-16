<template>
	<div class="detalis_order">
		<el-button type="primary" @click.native="mimaSubmits" style="background: #cab78c;border: none;margin-bottom: 10px">返回</el-button>
		<el-col :span="24" style="position: relative;background: #cab78c;height:48px;line-height: 48px;color: #fff;font-size: 16px;padding-left: 20px;">
			订单详情
		</el-col>
		<el-col :span="24" style="font-size: 16px;border-bottom: 1px dashed #cab78c;color: #616161;line-height: 55px;"><!-- :offset="4" -->
			<el-col :span="6" :offset="2">支付状态：{{table.status}}</el-col>
			<el-col :span="8" >订单编号：{{table.id}}</el-col>
			<el-col :span="8" >下单时间：{{table.createTime}}</el-col>
			<el-col :span="22" :offset="2" >交易号：{{table.tradeNo}}</el-col>
		</el-col>
		<el-col :span="24" style="position: relative;height:40px;line-height: 48px;color: #cab78c;font-size: 16px;padding-left: 20px;">
			收货信息
		</el-col>
		<el-col :span="24" style="font-size: 16px;border-bottom: 1px dashed #cab78c;color: #616161;line-height: 55px;"><!-- :offset="4" -->
			<el-col :span="6" :offset="2">收货人：{{table.consignee}}</el-col>
			<el-col :span="8" >收货人电话：{{table.mobile}}</el-col>
			<el-col :span="8" >邮编：{{table.zipCode}}</el-col>
			<el-button v-show="$route.params.index === 1" type="primary" @click="editDzBtn">修改</el-button>
			<el-col :span="22" :offset="2">收货地址：{{table.provinceName}}- {{table.cityName}} -{{table.countyName}} -{{table.address}}</el-col>
		</el-col>
		<!--<el-col :span="24" style="margin-top: 20px">-->
			<!--<el-col :span="13"style="margin-top: 20px;margin-left: 40px">收货信息</el-col>-->
			<!--<el-col :span="4"style="margin-top: 20px;margin-left: 40px">-->
			
			<!--</el-col>-->
			<!--<el-col :span="5"style="margin-top: 20px;margin-left: 40px">收货人：{{table.consignee}}</el-col>-->
			<!--<el-col :span="5"style="margin-top: 20px;margin-left: 40px">电话：{{table.mobile}}</el-col>-->
			<!--<el-col :span="24"style="margin-top: 20px;margin-left: 40px">收货地址：{{table.provinceName}}{{table.cityName}}{{table.countyName}}{{table.address}}</el-col>-->
		<!--</el-col>-->
		<el-col :span="24" style="position: relative;height:40px;line-height: 48px;color: #cab78c;font-size: 16px;padding-left: 20px;">
			买家信息
		</el-col>
		<el-col :span="24" style="font-size: 16px;border-bottom: 1px dashed #cab78c;color: #616161;line-height: 55px;"><!-- :offset="4" -->
			<el-col :span="6" :offset="2">账户名：{{table.coreUser.userName}}</el-col>
			<el-col :span="8" >昵称：{{table.coreUser.nickName}}</el-col>
			<el-col :span="8" >真实姓名：{{table.coreUser.realName}}</el-col>
			<el-col :span="8" :offset="2">身份证号码：{{table.coreUser.identityCard}}</el-col>
		</el-col>
		<el-col :span="24" class="order_information" style="margin-top: 10px;font-size: 16px;">商品信息</el-col>
		<el-col :span="24" class="commodity" style="background: #f4f2e8;height:36px;line-height: 36px;color: #616161;">
			<el-col :span="4" :offset="8">商品名称</el-col>
			<el-col :span="2" :offset="5">单价</el-col>
			<el-col :span="2">数量</el-col>
			<el-col :span="2">商品总价</el-col>
		</el-col>
		<el-col :span="24" class="commodity" v-for="item in table.orderGoods" style="margin-top: inherit;height:100px;padding-top: 20px;">
			<el-col :span="6">
					<img style="width: 80px;margin-left: 40px;" :src="item.picture">
				</el-col>
			<el-col :span="5" :offset="1">{{item.productName}}
				<el-col :span="24" class="size">
					<el-col :span="8" v-for="itemSta in item.attrName">{{itemSta.key}} {{itemSta.value}}</el-col>
				</el-col>
			</el-col>
			<el-col :span="2" :offset="5" style="margin-top: 20px">{{item.productPrice}}</el-col>
			<el-col :span="2" style="margin-top: 20px">{{item.quantity}}</el-col>
			<el-col :span="2" style="margin-top: 20px">{{item.productPrice*item.quantity}}</el-col>
		</el-col>
		<!--<el-col :span="24" class="commodity">支付金额：<span style="color:red;">￥{{table.totalMoney}}</span></el-col> -->
		<el-col :span="24" style="margin-top: 20px;color: #616161;line-height: 30px;">
			<el-col :span="8" :offset="1">商品总价：￥{{table.productValue}}</el-col>
			<el-col :span="8" class="">金豆抵扣金额：￥{{table.goldPrice}}</el-col>
			<el-col :span="6" class="">运费价格：￥{{table.expressValue}}</el-col>
			<el-col :span="8" :offset="1">优惠金额：￥{{table.discount}}</el-col>
			<el-col :span="8">支付方式：{{table.payMethod}}</el-col>
			<el-col :span="7">付款时间：{{table.payTime}}</el-col>
			<el-col :span="5" :offset="19" style="margin-top: 20px;">支付金额：<span style="color:red;font-size: 24px;">￥{{table.totalMoney}}</span></el-col>
		</el-col>
		<el-col :span="24" style="border-bottom: 1px dashed #cab78c;color: #616161;margin-top: 10px;"><!-- :offset="4" --></el-col>
		<!--<el-col :span="24" class="Payment_method">支付方式：{{table.payMethod}}</el-col> -->
		<!--<el-col class="time" :span="24">下单时间：{{table.payTime}}</el-col> -->
		<el-col :span="24" class="order_information" style="margin-top: 20px;font-size: 20px;color: #cab78c;">
			<el-button @click="seewl" type="text" style="font-size: 20px;color: #cab78c;">物流信息</el-button>
		</el-col>
		<el-col :span="24" class="order_information" style="margin-top: 20px">
			<el-button @click="seefy" type="text" style="font-size: 20px;color: #cab78c;">分佣详情</el-button>
		</el-col>
		<el-col :span="24" v-show="wlShow" v-for="item in wuliuinfo" style="margin-left: 40px;color: #aaa;margin-top: 10px">
			<el-col :span="24">{{item.AcceptStation}}</el-col>
			<el-col :span="24">{{item.AcceptTime}}</el-col>
		</el-col>
		<el-col :span="24" v-show="fyShow" style="margin-left: 40px;color: #aaa;margin-top: 10px">
			<el-col style="color: red;font-size: 18px;">提醒：确认收货之后分佣金额不会立即解冻！！！</el-col>
			<el-table :data="fenytable" border highlight-current-row style="width: 90%;">
			<el-table-column prop="nickName" label="用户名">
			</el-table-column>
			<el-table-column prop="totalMoney" label="分佣金额">
			</el-table-column>
			<el-table-column prop="commissionType" :formatter='formatterfy' label="分佣类型">
			</el-table-column>
			<el-table-column prop="status" :formatter='formatter' label="状态">
			</el-table-column>
			<el-table-column prop="remarks" label="备注">
			</el-table-column>
			<el-table-column prop="createTime" :formatter='formatterTime' label="创建时间">
			</el-table-column>
			<el-table-column label="发货时间">
				<template scope="scope">
					<div>{{deliveryTime}}</div>
				</template>
			</el-table-column>
			<el-table-column prop="thawingTime" :formatter='formatterjdTime' label="解冻时间">
			</el-table-column>
		</el-table>
		</el-col>
		<!-- <el-col :span="24" class="order_information" style="margin-top: 20px">发票信息</el-col>    deliveryTime
		<el-col :span="24" class="order_information" style="margin-top: 20px">操作历史</el-col>
		<el-col :span="24" class="footerr_text"><span style="color:red">1245454541</span> 于2014-02-09 11:09:18 订单当前状态：提交订单 下一状态：等待收货</el-col>
		<el-col :span="24" class="footerr_text"><span style="color:red">admin</span> 于2014-02-09 11:09:18 订单当前状态：提交订单 下一状态：等待收货</el-col> -->
		<el-dialog title="修改地址" v-model="editFormVisible" :close-on-click-modal="false" >
			<el-form :model="addfrom" label-width="160px">
				<el-form-item label="收货人">
					<el-input v-model="addfrom.consignee" type="text"></el-input>
				</el-form-item>
				<el-form-item label="手机号">
					<el-input v-model="addfrom.mobile" type="text"></el-input>
				</el-form-item>
				<el-form-item label="收货地址">
					<el-cascader
				    :options="options"
				    v-model="addfrom.areaId"
				    @change="handleChange">
				  </el-cascader>
				</el-form-item>
				<el-form-item label="详细地址">
					<el-input v-model="addfrom.address" type="text"></el-input>
				</el-form-item>
			</el-form>
			<div slot="footer" class="dialog-footer" style="text-align: center;">
				<el-button type="primary" @click.native="editSubmit">保存</el-button>
				<el-button type="primary" @click.native="editFormVisible = false">关闭</el-button>
			</div>
		</el-dialog>
	</div>
</template>
<script>
	import { state } from '../../vuex/state'
	import util from '../../common/js/util'
	import {baseUrl , editUser, addUser ,cityData3} from '../../api/api';

	export default {
		data() {
			return {
				deliveryTime:'',
				table:{},
				wuliuinfo:[],
				fenytable:[],
				wlShow:false,
				fyShow:true,
				editFormVisible:false,
				addfrom:{
					areaId:[],
					consignee:'',
					mobile:'',
					address:''
				},
				options: cityData3,
				isareaId:'',
				sName:'',
				shiName:'',
				qName:''
			}
		},
		methods: {
			editDzBtn(){
				this.editFormVisible = true
				this.addfrom.consignee = this.table.consignee
				this.addfrom.mobile = this.table.mobile
				this.addfrom.address = this.table.address
				// console.log(this.table)
			},
			editSubmit(){
				const _this = this
				// console.log(this.addfrom)
				const params = {
					id:this.table.id,
					consignee:this.addfrom.consignee,
					mobile:this.addfrom.mobile,
					zipCode:'000000',
					province:this.addfrom.areaId[0],
					provinceName:this.sName,
					city:this.addfrom.areaId[1],
					cityName:this.shiName,
					county:this.addfrom.areaId[2],
					countyName:this.qName,
					address:this.addfrom.address
				}
				// console.log(params)
				$.ajax({
                    type:'POST',
                    dataType:'json',
                    url:baseUrl+"/api/orderMall/updateOrderAddress",
                    data:JSON.stringify(params),
                    contentType:'application/json;charset=utf-8',
                    success:function(data){
                    	// console.log(data)
                    	if(data.code === 1){
                    		_this.editFormVisible = false
                    		_this.getlist()
                    	}
                    	
                    }
                });
			},
			handleChange(value) {
		        // console.log(value);
		        // console.log(this.addfrom.areaId)
		        this.isareaId = value[value.length-1]
		        // console.log(this.isareaId)
		        this.areaId = value[0]
		        this.cityId = value[1]
		        this.provinceId = value[2]
		        let arry = []
		        let arry1 = []
		        for(var i = 0;i<cityData3.length;i++){
		        	if(value[0] === cityData3[i].value){
		        		// console.log(cityData3[i].children)
		        		this.sName = cityData3[i].label
		        		arry = cityData3[i].children
		        	}
		        }
		        for(var i = 0;i<arry.length;i++){
		        	if(value[1] === arry[i].value){
		        		// console.log(arry[i].label)
		        		this.shiName = arry[i].label
		        		arry1 = arry[i].children
		        	}
		        }
		        for(var i = 0;i<arry1.length;i++){
		        	if(value[2] === arry1[i].value){
		        		// console.log(arry1[i].label)
		        		this.qName = arry1[i].label
		        	}
		        }
		    },
			seewl(){
				this.wlShow = true
				this.fyShow = false
			},
			seefy(){
				this.fyShow = true
				this.wlShow = false
			},
			getlist(){
				const _this = this
				const params = {
					pageNum:1,
					size:10,
                    storeId:localStorage.getItem("storeId"),
					orderId:this.$route.params.id,
					expno:''
				}
				$.ajax({
                    type:'POST',
                    dataType:'json',
                    url:baseUrl+"/api/orderMall/selectListAll",
                    data:JSON.stringify(params),
                    contentType:'application/json;charset=utf-8',
                    success:function(data){
                    	console.log(data)
                    	const info = data.data
                    	_this.table = info.list[0]
                    	const arryDZ = [
                    		_this.table.province,
                    		_this.table.city,
                    		_this.table.county
                    	]
                    	// console.log(_this.table)
                    	// console.log(_this.table.province)
                    	// console.log(_this.table.city)
                    	// console.log(_this.table.county)
                    	_this.addfrom.areaId = arryDZ
                    	// console.log(arryDZ) createTime
                    	if(_this.table.createTime !== null){
                    		_this.table.createTime = new Date(_this.table.createTime).toLocaleString()
                    	}
                    	if(_this.table.payTime !== null){
                    		_this.table.payTime = new Date(_this.table.payTime).toLocaleString()
                    	}
                    	if(_this.table.orderGoods[0].deliveryTime === null){
                    		_this.deliveryTime = '卖家未发货'
                    	}else{
                    		_this.deliveryTime = new Date(_this.table.orderGoods[0].deliveryTime).toLocaleString()
                    	}
                    	console.log(_this.deliveryTime)
                    	for(var i = 0;i<_this.table.orderGoods.length;i++){
                    		_this.table.orderGoods[i].attrName = JSON.parse(_this.table.orderGoods[i].attrName)
                    		// console.log(_this.table.orderGoods[i].attrName)
                    	}
                    	if(_this.table.payMethod === '0'){
                    		_this.table.payMethod = '微信支付'
                    	}else if(_this.table.payMethod === '1'){
                    		_this.table.payMethod = '支付宝支付'
                    	}else if(_this.table.payMethod === '2'){
                    		_this.table.payMethod = '银联支付'
                    	}else if(_this.table.payMethod === '3'){
                    		_this.table.payMethod = '余额支付'
                    	}else if(_this.table.payMethod === '4'){
                    		_this.table.payMethod = '余额金豆混合支付'
                    	}else if(_this.table.payMethod === '5'){
                    		_this.table.payMethod = '金豆支付'
                    	}
                    	if(_this.table.status === 1){
                    		_this.table.status = '待支付'
                    	}else if(_this.table.status === 2){
                    		_this.table.status = '支付成功'
                    	}else if(_this.table.status === 3){
                    		_this.table.status = '支付失败'
                    	}else if(_this.table.status === 4){
                    		_this.table.status = '已完成'
                    	}else if(_this.table.status === 5 || _this.table.status === 7){
                    		_this.table.status = '已取消'
                    	}
                    	// console.log(_this.table)
                    	// console.log(data)
                    	for(var i = 0;i<_this.table.orderGoods.length;i++){
                    		if(_this.table.orderGoods[i].picture === null){
                    			_this.table.orderGoods[i].picture = 'icone.png'
                    		}
                    		const params = {
								expno:_this.table.orderGoods[i].expno,
								expressCode:_this.table.orderGoods[i].expressCode,
								expressName:_this.table.orderGoods[i].expressName,
								picture:_this.table.orderGoods[i].picture
							}
							// console.log(_this.table.orderGoods[i].province+_this.table.orderGoods[i].city+_this.table.orderGoods[i].county)
							// console.log(params)
							_this.souTraces(params)
                    	}
                    	_this.fenyList(_this.table.id)
                    }
                });
			},
			// 物流信息
			souTraces(params){
				const _this = this
				// console.log(params)
				$.ajax({
                    type:'POST',
                    dataType:'json',
                    url:baseUrl+"/api/orderMall/orderTraces",
                    data:JSON.stringify(params),
                    contentType:'application/json;charset=utf-8',
                    success:function(data){
                    	// console.log(data)
                    	if(data.data !== null){
                    		_this.wuliuinfo = data.data.jsonArray.reverse()
                    	}
                    	
                    }
                });
			},
			// 分佣明细
			fenyList(id){
				const _this = this
				const params = {
					orderId:id
				}
				// console.log(params)
				$.ajax({
                    type:'POST',
                    dataType:'json',
                    url:baseUrl+"/api/orderMall/selectCentsByOrderId",
                    data:JSON.stringify(params),
                    contentType:'application/json;charset=utf-8',
                    success:function(data){
                    	// console.log(data)
                    	_this.fenytable = data.data
                    	// _this.wuliuinfo = data.data.jsonArray.reverse()
                    }
                });
			},
			formatterTime(row, column){
				let curTime = row.createTime;
                curTime = new Date(curTime).toLocaleString()
                return curTime
			},
			formatterjdTime(row, column){
				let curTime = row.thawingTime;
				if(curTime === null){
					return
				}
                curTime = new Date(curTime).toLocaleString()
                return curTime
			},
			formatter(row, column){
				let type = ''
				if(row.status === 1){
					type = '冻结中  '
				}else if(row.status === 2){
					type = '已解冻 '
				}else if(row.status === 3){
					type = '退款作废'
				}
				return type
			},
			formatterfy(row, column){
				let type = ''
				if(row.commissionType === 0){
					type = '平台分佣线  '
				}else if(row.commissionType === 1){
					type = '直推奖'
				}else if(row.commissionType === 2){
					type = '对碰奖 '
				}else if(row.commissionType === 3){
					type = '培育奖'
				}else if(row.commissionType === 4){
					type = '荣誉奖'
				}else if(row.commissionType === 5){
					type = '店铺三级分销  '
				}else if(row.commissionType === 6){
					type = '店铺团队奖'
				}else if(row.commissionType === 7){
					type = '店铺平级奖 '
				}else if(row.commissionType === 8){
					type = '店铺加权分红'
				}else if(row.commissionType === 9){
					type = '商品货款'
				}else if(row.commissionType === 10){
					type = '花说分佣线奖'
				}
				return type
			},
			mimaSubmits(){
                window.history.go(-1);
			}
		},
		mounted() {
			this.getlist()
		}
	}

</script>
<style>
	.footerr_text{
		margin-left: 25px;
		margin-top: 20px;
	}
	.time{
		margin-left: 20px;
		margin-top: 20px;
		padding-bottom: 10px;
	}
	.Payment_method{
		border-top: 1px solid #eee;
		border-bottom: 1px solid #eee;
		margin-top: 20px;
		padding-bottom: 10px;
		padding-top: 10px;
		padding-left: 20px;
	}
	.size{
		color: #bbb;
		padding-top: 20px
	}
	.tool_style{
		padding-left: 26px;
		margin-top: 10px
	}
	.commodity_first{
		width: 50px;
		height: 50px;
		border: 1px solid #ddd;
	}
	.commodity{
		border-bottom: 1px solid #ddd;
		padding-left: 20px;
		padding-bottom: 10px;
	}
	.detalis_order{
		margin-left: 20px;
		width: 100%;
		height:1600px;
		border-radius: 10px;
	}
	.detalis_order_top{
		height:50px;
		line-height: 50px;
		font-size: 18px;
		color: #20a0ff;
		margin-left: 20px;
	}
	.order_information{
		height:40px;
		line-height: 40px;
		font-size: 20px;
		color: #cab78c;
		padding-left:20px;

	}
</style>
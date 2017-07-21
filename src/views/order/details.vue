<template>
	<div class="detalis_order">
		<div class="detalis_order_top">订单详情</div>
		<el-col :span="24" style="margin-top: 20px"><!-- :offset="4" -->
			<el-col :span="4" :offset="1">支付状态：{{table.status}}</el-col>
			<el-col :span="6" :offset="2">订单编号：{{table.id}}</el-col>
			<el-col :span="6" :offset="2">下单时间：{{table.payTime}}</el-col>
		</el-col>
		<el-col :span="24" class="order_information" style="margin-top: 20px">订单信息</el-col>
		<el-col :span="24" class="commodity">
			<el-col :span="4" :offset="8">商品名称</el-col>
			<el-col :span="2" :offset="5">单价</el-col>
			<el-col :span="2">数量</el-col>
			<el-col :span="2">商品总价</el-col>
		</el-col>
		<el-col :span="24" class="commodity" v-for="item in table.orderGoods">
			<el-col :span="6">
					<img style="width: 80px;margin-left: 40px;" :src="item.picture">
				</el-col>
			<el-col :span="5" :offset="1">{{item.productName}}
				<el-col :span="24" class="size">
					<el-col :span="8" v-for="itemSta in item.attrName">{{itemSta.key}}：{{itemSta.value}}</el-col>
				</el-col>
			</el-col>
			<el-col :span="2" :offset="5" style="margin-top: 20px">{{item.productPrice}}</el-col>
			<el-col :span="2" style="margin-top: 20px">{{item.quantity}}</el-col>
			<el-col :span="2" style="margin-top: 20px">{{item.productPrice*item.quantity}}</el-col>
		</el-col>
		<el-col :span="24" class="commodity">支付金额：<span style="color:red;">￥{{table.totalMoney}}</span></el-col> 
		<el-col :span="24" style="color:#aaa;margin-top: 20px">
			<el-col :span="8" :offset="1">商品总价：￥{{table.productValue}}</el-col>
			<el-col :span="8" class="">金豆抵扣金额：￥{{table.goldPrice}}</el-col>
			<el-col :span="6" class="">运费价格：￥{{table.expressValue}}</el-col>
		</el-col> 
		<el-col :span="24" class="Payment_method">支付方式：{{table.payMethod}}</el-col> 
		<el-col class="time" :span="24">下单时间：{{table.payTime}}</el-col> 
		<el-col :span="24" class="order_information" style="margin-top: 20px">物流信息</el-col>
		<el-col :span="24" v-for="item in wuliuinfo" style="margin-left: 40px;color: #3e3e3e;margin-top: 10px">
			<el-col :span="24">{{item.AcceptStation}}</el-col>
			<el-col :span="24">{{item.AcceptTime}}</el-col>
		</el-col>
		<!-- <el-col :span="24" class="order_information" style="margin-top: 20px">发票信息</el-col>
		<el-col :span="24" class="order_information" style="margin-top: 20px">操作历史</el-col>
		<el-col :span="24" class="footerr_text"><span style="color:red">1245454541</span> 于2014-02-09 11:09:18 订单当前状态：提交订单 下一状态：等待收货</el-col>
		<el-col :span="24" class="footerr_text"><span style="color:red">admin</span> 于2014-02-09 11:09:18 订单当前状态：提交订单 下一状态：等待收货</el-col> -->
	</div>
</template>
<script>
	import { state } from '../../vuex/state'
	import util from '../../common/js/util'
	import {baseUrl , editUser, addUser } from '../../api/api';

	export default {
		data() {
			return {
				table:{},
				wuliuinfo:[]
			}
		},
		methods: {
			getlist(){
				const _this = this
				const params = {
					pageNum:1,
					size:10,
					storeId:state.storeId,
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
                    	const info = data.data
                    	_this.table = info.list[0]
                    	_this.table.payTime = new Date(_this.table.payTime).toLocaleString()
                    	for(var i = 0;i<_this.table.orderGoods.length;i++){
                    		_this.table.orderGoods[i].attrName = JSON.parse(_this.table.orderGoods[i].attrName)
                    		console.log(_this.table.orderGoods[i].attrName)
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
                    	}else if(_this.table.status === 5){
                    		_this.table.status = '已删除'
                    	}
                    	console.log(_this.table)
                    	console.log(_this.table.orderGoods)
                    	for(var i = 0;i<_this.table.orderGoods.length;i++){
                    		const params = {
								expno:_this.table.orderGoods[i].expno,
								expressCode:_this.table.orderGoods[i].expressCode
							}
							_this.souTraces(params)
                    	}
                    }
                });
			},
			souTraces(params){
				const _this = this
				$.ajax({
                    type:'POST',
                    dataType:'json',
                    url:baseUrl+"/api/orderMall/orderTraces",
                    data:JSON.stringify(params),
                    contentType:'application/json;charset=utf-8',
                    success:function(data){
                    	console.log(data)
                    	_this.wuliuinfo = data.data.jsonArray
                    }
                });
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
		width: 1130px;
		margin-left: 20px;
		margin-top: 20px;
		padding-bottom: 10px;
	}
	.Payment_method{
		border-top: 1px solid #eee;
		border-bottom: 1px solid #eee;
		/*color: #aaa;*/
		width: 1130px;
		margin-left: 20px;
		margin-top: 20px;
		padding-bottom: 10px;
		padding-top: 10px;
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
		margin-left: 20px;
	}
	.commodity{
		border-bottom: 1px solid #ddd;
		width: 1130px;
		margin-left: 20px;
		margin-top: 20px;
		padding-bottom: 10px;
	}
	.detalis_order{
		margin-left: 100px;
		margin-top: 40px;
		width: 1200px;
		height:800px;
		border: 1px solid #aaa;
		border-radius: 10px;
	}
	.detalis_order_top{
		border-bottom: 1px solid #aaa;
		width: 1110px;
		height:50px;
		line-height: 50px;
		font-size: 18px;
		color: #20a0ff;
		margin-left: 20px;
		padding-left:20px;
	}
	.order_information{
		border: 1px solid #ddd;
		width:1130px;
		height:40px;
		line-height: 40px;
		font-size: 14px;
		color: #20a0ff;
		margin-left: 20px;
		padding-left:20px;
	}
</style>
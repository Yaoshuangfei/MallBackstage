<template>
	<div class="detalis_order">
		<div class="detalis_order_top">订单详情</div>
		<el-col :span="24" style="margin-top: 20px"><!-- :offset="4" -->
			<el-col :span="4" :offset="1">订单状态：订单取消</el-col>
			<el-col :span="6" :offset="2">订单编号：10254125555547852114555</el-col>
			<el-col :span="6" :offset="2">下单时间：2018-02-06 20:52:44</el-col>
		</el-col>
		<el-col :span="24" class="order_information" style="margin-top: 20px">订单信息</el-col>
		<el-col :span="24" class="commodity">
			<el-col :span="4" :offset="8">商品名称</el-col>
			<el-col :span="2" :offset="6"  style="margin-top: 0px">单价</el-col>
			<el-col :span="2">数量</el-col>
			<el-col :span="2">商品总价</el-col>
		</el-col>
		<el-col :span="24" class="commodity">
			<el-col :span="4" class="commodity_first"></el-col>
			<el-col :span="10" :offset="1" style="padding-left: 20px">商品时间地哦啊是分散地方撒地方和萨芬的权威热情为名称
			<div class="size">尺码：L</div>
			</el-col>
			<el-col :span="2" :offset="5" class="tool_style">122</el-col>
			<el-col :span="2" class="tool_style">1</el-col>
			<el-col :span="2" class="tool_style">232</el-col>
		</el-col>
		<el-col :span="24" class="commodity">支付金额：<span style="color:red;">￥264.00</span></el-col> 
		<el-col :span="24" style="color:#aaa;margin-top: 20px">
			<el-col :span="8" :offset="1">商品总价：￥122.00</el-col>
			<el-col :span="8" class="">无忧宝支付：￥40.00</el-col>
			<el-col :span="6" class="">运费价格：￥10.00</el-col>
		</el-col> 
		<el-col :span="24" class="Payment_method">支付方式：</el-col> 
		<el-col class="time" :span="24">下单时间：2018-02-06 20:52:44</el-col> 
		<el-col :span="24" class="order_information" style="margin-top: 20px">物流信息</el-col>
		<el-col :span="24" class="order_information" style="margin-top: 20px">发票信息</el-col>
		<el-col :span="24" class="order_information" style="margin-top: 20px">操作历史</el-col>
		<el-col :span="24" class="footerr_text"><span style="color:red">1245454541</span> 于2014-02-09 11:09:18 订单当前状态：提交订单 下一状态：等待收货</el-col>
		<el-col :span="24" class="footerr_text"><span style="color:red">admin</span> 于2014-02-09 11:09:18 订单当前状态：提交订单 下一状态：等待收货</el-col>
	</div>
</template>
<script>
	import { state } from '../../vuex/state'
	import util from '../../common/js/util'
	import {baseUrl , editUser, addUser } from '../../api/api';

	export default {
		data() {
			return {}
		},
		methods: {
		},
		mounted() {
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
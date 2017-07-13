<template>
	<el-row :gutter="10">
	  <el-col :xs="24" :sm="24" :md="24" :lg="24" style="margin-top: 50px;">
	  	<el-col :xs="3" :sm="3" :md="3" :lg="3">
	  		<img style="width: 100px;margin-left: 40px;border-radius: 50px;" src="http://121.43.178.109:8080/ser/ATTACHMENT/ATTACHMENT/2e97868c-4029-411c-ac61-3c0a2d6ff035.png">
	  	</el-col>
	  	<el-col :xs="8" :sm="6" :md="4" :lg="8">
	  		<el-col :xs="24" :sm="24" :md="24" :lg="24">
	  			<h1>商家后台管理中心</h1>
	  		</el-col>
	  		<el-col :xs="24" :sm="24" :md="24" :lg="24">
	  			卖家信用：100
	  		</el-col>
	  	</el-col>
	  	<el-col :xs="8" :sm="6" :md="4" :lg="8">
	  		<el-col :xs="24" :sm="24" :md="24" :lg="24">
	  			<h1>商家动态评分：100</h1>
	  		</el-col>
	  		<el-col :xs="24" :sm="24" :md="24" :lg="24">
	  			描述相符：100
	  		</el-col>
	  		<el-col :xs="24" :sm="24" :md="24" :lg="24">
	  			服务态度：100
	  		</el-col>
	  		<el-col :xs="24" :sm="24" :md="24" :lg="24">
	  			发货速度：100
	  		</el-col>
	  	</el-col>
	  </el-col>
	  <el-col :xs="24" :sm="24" :md="24" :lg="24" style="margin-top: 50px;margin-left: 40px">
	  	<el-col :xs="12" :sm="12" :md="12" :lg="11">
	  		<el-col :xs="24" :sm="24" :md="24" :lg="24">
	  			<h1>商品提示</h1>
	  		</el-col>
	  		<el-col :xs="24" :sm="24" :md="24" :lg="24">
	  			<el-col :xs="24" :sm="24" :md="24" :lg="24">
	  				<el-col :xs="12" :sm="12" :md="12" :lg="12">
	  					商家商品（{{num}}）
	  				</el-col>
	  				<el-col :xs="12" :sm="12" :md="12" :lg="12">
	  					商品评价（{{num}}）
	  				</el-col>
	  			</el-col>
	  			<el-col :xs="24" :sm="24" :md="24" :lg="24" style="margin-top: 20px">
	  				<el-col :xs="12" :sm="12" :md="12" :lg="12">
	  					出售中的商品（{{num}}）
	  				</el-col>
	  				<el-col :xs="12" :sm="12" :md="12" :lg="12">
	  					违规下架的商品（{{num}}）
	  				</el-col>
	  			</el-col>
	  		</el-col>
	  		<el-col :xs="24" :sm="24" :md="24" :lg="24">
	  		</el-col>
	  		<el-col :xs="24" :sm="24" :md="24" :lg="24">
	  			<h1>交易提醒</h1>
	  		</el-col>
	  		<el-col :xs="24" :sm="24" :md="24" :lg="24">
	  			近期出售：所有订单
	  		</el-col>
	  		<el-col :xs="24" :sm="24" :md="24" :lg="24" style="margin-top: 20px">
	  			<el-col :xs="8" :sm="8" :md="8" :lg="6">
	  				待收货（{{num}}）
	  			</el-col>
	  			<el-col :xs="8" :sm="8" :md="8" :lg="6">
	  				待发货（{{num}}）
	  			</el-col>
	  			<el-col :xs="8" :sm="8" :md="8" :lg="6">
	  				待评价（{{num}}）
	  			</el-col>
	  			<el-col :xs="8" :sm="8" :md="8" :lg="6">
	  				退货（{{num}}）
	  			</el-col>
	  		</el-col>
	  	</el-col>
	  	<el-col :xs="12" :sm="12" :md="12" :lg="12">
	  		<el-col :xs="24" :sm="24" :md="24" :lg="24">
	  			<h1>我的分销商</h1>
	  		</el-col>
	  		<el-col :xs="24" :sm="24" :md="24" :lg="24">
	  			商品提示
	  		</el-col>
	  	</el-col>
	  </el-col>
	</el-row>
</template>

<script>
	import { state } from '../../vuex/state'
	import util from '../../common/js/util'
	import {baseUrl , editUser, addUser } from '../../api/api';

	export default {
		data() {
			return {
				num:0,
			}
		},
		methods: {
			
		},
		mounted() {

		}
	}

</script>

<style>
	
</style>
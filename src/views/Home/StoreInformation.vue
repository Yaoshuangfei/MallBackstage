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
	  			<el-col :xs="12" :sm="12" :md="12" :lg="12">
	  			店主：{{name}}
	  			</el-col>
	  			<el-col :xs="12" :sm="12" :md="12" :lg="12">
	  			联系电话：112345155565
	  			</el-col>
	  			<el-col :xs="12" :sm="12" :md="12" :lg="12">
	  			好评率：{{favorableRate}}
	  			</el-col>
	  		</el-col>
	  	</el-col>
	  	<el-col :xs="8" :sm="6" :md="4" :lg="8">
	  		<el-col :xs="24" :sm="24" :md="24" :lg="24">
	  			<h1>商家动态评分：10</h1>
	  		</el-col>
	  		<el-col :xs="24" :sm="24" :md="24" :lg="24">
	  			描述相符：{{goodsScore}}
	  		</el-col>
	  		<el-col :xs="24" :sm="24" :md="24" :lg="24">
	  			服务态度：{{sellerScore}}
	  		</el-col>
	  		<el-col :xs="24" :sm="24" :md="24" :lg="24">
	  			发货速度：{{logisticsScore}}
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
	  					上架商品（{{num}}）
	  				</el-col>
	  				<el-col :xs="12" :sm="12" :md="12" :lg="12">
	  					差评（{{cpnum}}）
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
	  				待收货（{{dshnum}}）
	  			</el-col>
	  			<el-col :xs="8" :sm="8" :md="8" :lg="6">
	  				待发货（{{dfhnum}}）
	  			</el-col>
	  			<el-col :xs="8" :sm="8" :md="8" :lg="6">
	  				待评价（{{dpjnum}}）
	  			</el-col>
	  			<el-col :xs="8" :sm="8" :md="8" :lg="6">
	  				退货（{{thnum}}）
	  			</el-col>
	  		</el-col>
	  	</el-col>
	  	<el-col :xs="12" :sm="12" :md="12" :lg="12">
	  		<el-col :xs="24" :sm="24" :md="24" :lg="24">
	  			<h1>我的分销商</h1>
	  		</el-col>
	  		<el-col :xs="12" :sm="12" :md="12" :lg="12" style="border: 1px solid #ddd;min-height: 100px;">
	  			<el-col :xs="24" :sm="24" :md="24" :lg="24" style="margin-top: 20px;">
	  				<el-col :xs="5" :sm="5" :md="5" :lg="5">
	  				<img style="width:50px;border-radius: 50px;" src="http://121.43.178.109:8080/ser/ATTACHMENT/ATTACHMENT/2e97868c-4029-411c-ac61-3c0a2d6ff035.png">
	  				</el-col>
	  				<el-col :xs="5" :sm="5" :md="5" :lg="5" style="margin-top: 15px;">嘻嘻嘻</el-col>
	  				<el-col :xs="5" :sm="5" :md="5" :lg="5" style="margin-top: 15px;">一级分销</el-col>
	  				<el-col :xs="5" :sm="5" :md="5" :lg="5" style="margin-top: 15px;">查看更多</el-col>
	  			</el-col>
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
				name:'',

				favorableRate:'',
				goodsScore:'',
				sellerScore:'',
				logisticsScore:'',

				num:0,
				xsznum:0,
				xjnum:0,
				wgnum:0,
				cpnum:0,
				dshnum:0,
				dfhnum:0,
				dpjnum:0,
				thnum:0
			}
		},
		methods: {
			getlist(){
				const _this = this
				$.ajax({
	                type:'GEt',
	                dataType:'json',
	                // url:'http://192.168.0.106:8080/api/store/selectStore',
	                url:baseUrl+'/api/store/selectStore',
	                data:{},
	                contentType:'application/json;charset=utf-8',
	                success:function(data){
	                	const info = data.data
	                  	console.log(data.data)
	                  	for(var i = 0;i<info.goodsList.length;i++){
	                  		if(info.goodsList[i].saleStatus === 1){
	                  			_this.xsznum = info.goodsList[i].countGroup
	                  		}else if(info.goodsList[i].saleStatus === 2){
	                  			_this.xjnum = info.goodsList[i].countGroup
	                  		}else  if(info.goodsList[i].saleStatus === 4){
	                  			_this.wgnum = info.goodsList[i].countGroup
	                  		}else if(info.goodsList[i].saleStatus === 5){
	                  			_this.cpnum = info.goodsList[i].countGroup
	                  		}
	                  	}
	                  	for(var i = 0;i<info.orderMalls.length;i++){
	                  		if(info.orderMalls[i].orderStatus === 5){
	                  			_this.dshnum = info.orderMalls[i].countGroup
	                  		}else if(info.orderMalls[i].orderStatus === 2){
	                  			_this.dfhnum = info.orderMalls[i].countGroup
	                  		}else  if(info.orderMalls[i].orderStatus === 6){
	                  			_this.dpjnum = info.orderMalls[i].countGroup
	                  		}else if(info.orderMalls[i].orderStatus === 4){
	                  			_this.thnum = info.orderMalls[i].countGroup
	                  		}
	                  	}
	                  	_this.name = info.store.name
	                  	_this.favorableRate = info.store.favorableRate
	                  	_this.goodsScore = info.store.goodsScore
	                  	_this.sellerScore = info.store.sellerScore
	                  	_this.logisticsScore = info.store.logisticsScore
	                }
	            })
			}
		},
		mounted() {
			this.getlist()
		}
	}

</script>

<style>
	
</style>
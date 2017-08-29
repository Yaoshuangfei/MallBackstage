<template>
	<section>
		<el-col :span="24" style="position: relative;background: #cab78c;height:48px;line-height: 48px;color: #fff;font-size: 16px;padding-left: 20px;margin-bottom: 20px;">
			分享规则管理
		</el-col>
		<el-col :span="24" style="width:100%;margin-top: 80px;padding-left: 60px;">
			<el-col :span="3" style="margin-top: 8px">最低提现额度</el-col>
			<el-col :span="4">
				<el-input v-model="minPrice"></el-input>
			</el-col>
			<el-col :span="1" style="margin-top: 8px">元</el-col>
		</el-col>
		<el-col :span="24" style="width:100%;margin-top: 40px;padding-left: 60px;">
			<el-col :span="3" style="margin-top: 8px">提现时间</el-col>
			<el-col :span="20">
				<el-checkbox-group v-model="roleId" @change = "click">
				    <el-checkbox v-for="city in ruleAll" :label="city.id" :key="city.id">{{city.name}}</el-checkbox>
				</el-checkbox-group>
				<!-- <el-radio-group v-model="roleId" @change = "click">
			    	<el-radio-button  v-for="item in ruleAll" :label="item.id">{{item.name}}</el-radio-button>
			    </el-radio-group> -->
			</el-col>
		</el-col>
		<el-col :span="24" style="width:100%;margin-top: 40px;padding-left: 60px;">
			<el-col :span="3" style="margin-top: 8px">手续费</el-col>
			<el-col :span="4">
				<el-input v-model="adminDiscount" type="number"></el-input>
			</el-col>
			<el-col :span="1"  style="margin-top: 8px">%</el-col>
		</el-col>
		<el-col :span="24" style="width:100%;margin-top: 40px;padding-left: 200px">
			<el-button v-if="bcandEdit" type="primary" @click="onSubmit">保存</el-button>
			<el-button v-else type="primary" @click="eidtSubmit">修改</el-button>
		</el-col>
	</section>
</template>

<script>
	import { state } from '../../vuex/state'
	import util from '../../common/js/util'
	import {baseUrl , editUser, addUser } from '../../api/api';

	export default {
		data() {
			return {
				bcandEdit:false,
				minPrice:'',
				adminDiscount:'',
				roleId:[],
				allocationId:'',
				ruleAll:[
					{
						id:'1',
						name:'星期一'
					},
					{
						id:'2',
						name:'星期二'
					},
					{
						id:'3',
						name:'星期三'
					},
					{
						id:'4',
						name:'星期四'
					},
					{
						id:'5',
						name:'星期五'
					},
					{
						id:'6',
						name:'星期六'
					},
					{
						id:'7',
						name:'星期七'
					}
				]
			}
		},
		methods: {
			click(){

			},
			onSubmit(){
				const _this = this
				const params = {
					withdrawalsWeekStr:this.roleId.toString(),
					adminDiscount:parseInt(this.adminDiscount),
					minPrice:parseInt(this.minPrice),
					storeId:state.storeId
				}
				console.log(params)
				$.ajax({
	                type:'POST',
	                dataType:'json',
	                url:baseUrl+"/api/commissionAllocation/add/update",
	                data:JSON.stringify(params),
	                contentType:'application/json;charset=utf-8',
	                success:function(data){
	                  	console.log(data)
	                  	_this.getlist()
	                }
	            });
			},
			eidtSubmit(){
				const _this = this
				const params = {
					withdrawalsWeekStr:this.roleId.toString(),
					adminDiscount:parseInt(this.adminDiscount),
					minPrice:parseInt(this.minPrice),
					storeId:state.storeId,
					allocationId:this.allocationId
				}
				console.log(params)
				$.ajax({
	                type:'POST',
	                dataType:'json',
	                url:baseUrl+"/api/commissionAllocation/add/update",
	                data:JSON.stringify(params),
	                contentType:'application/json;charset=utf-8',
	                success:function(data){
	                  	console.log(data)
	                  	_this.getlist()
	                }
	            });
			},
			getlist(){
				const _this = this
				$.ajax({
	                type:'POST',
	                dataType:'json',
	                url:baseUrl+"/api/commissionAllocation/selectOne",
	                data:JSON.stringify({}),
	                contentType:'application/json;charset=utf-8',
	                success:function(data){
	                  	console.log(data)
	                  	if(data.data === null){
	                  		_this.bcandEdit = true
	                  		return
	                  	}
	                  	_this.roleId = data.data.withdrawalsWeekStr.split(',')
	                  	_this.adminDiscount = data.data.adminDiscount
	                  	_this.minPrice = data.data.minPrice
	                  	_this.allocationId = data.data.allocationId
	                }
	            });
			}
		},
		mounted() {
			this.getlist();
		}
	}

</script>

<style>
	.agen_div{
		border: 1px solid #aaa;
		width:200px;
		height: 300px;

	}
	.agen_div div:nth-child(1){
		border: 1px solid #aaa;
		width:100px;
		height: 100px;
		border-radius: 50px;
		margin-left: 45px;
		margin-top: 20px;
	}
	.agen_div div:nth-child(2){
		width: 200px;
		text-align: center;
		margin-top: 20px;
	}
	.agen_div div:nth-child(3){
		width: 200px;
		text-align: center;
		margin-top: 20px;
	}
	.agen_div div:nth-child(4){
		width: 200px;
		text-align: center;
		margin-top: 20px;
	}
</style>
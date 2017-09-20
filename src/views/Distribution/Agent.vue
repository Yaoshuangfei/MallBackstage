<template>
	<section>
		<!--工具条-->
		<el-col :span="24" style="position: relative;background: #cab78c;height:48px;line-height: 48px;color: #fff;font-size: 16px;padding-left: 20px;margin-bottom: 20px;">
			我的分销商（{{FxsList.length}}）
		</el-col>
		<!--<el-col :span="24" style="padding-bottom: 0px;background: #fff">-->
			<!--<h3></h3>-->
		<!--</el-col>-->
		<el-col :span="24" class="toolbar" style="padding-bottom: 0px;background: #fff">
			<el-form :inline="true">
				<el-form-item label="条件">
					<el-select v-model="condition" clearable>
				      <el-option v-for="item in states" :label="item.label" :value="item.value">
				      </el-option>
				    </el-select>
				</el-form-item>
				<!-- <el-form-item label="搜索类型">
				    <el-select v-model="filters.type" clearable>
				      <el-option v-for="item in options" :label="item.label" :value="item.value">
				      </el-option>
				    </el-select>
				</el-form-item> -->
				<el-form-item>
				    <el-input v-model="value"></el-input>
				</el-form-item>
				<el-form-item style="margin-right: inherit;">
					<el-button type="primary" v-on:click="getlist">查询</el-button>
				</el-form-item>
			</el-form>
		</el-col>
		<el-col :span="24" style="padding-bottom: 0px;background: #fff">
			<el-col :xs="6" :sm="6" :md="6" :lg="6" v-for="item in FxsList" style='margin-bottom: 20px'>
				<div class="agen_div">
					<div>
						<img style="width: 100px;height:100px;border-radius: 50%;" :src="item.coreUser.headImg" v-if="item.coreUser.headImg !== null">
						<img v-else style="width: 100px;height:100px;border-radius: 50%;" src="http://resources.51att.cn/ATTACHMENT/ATTACHMENT/1bccc3cf-8d44-4482-84e1-82d84d56e25c.png">
					</div>
					<div>{{item.coreUser.nickName}}</div>
					<div>{{item.shopRoleName}}</div>
					<div>
						<router-link :to="{ name: '查看下级', params: { id: item.id , name:item.coreUser.nickName}}">
							<el-button  type="text">查看下级</el-button>
						</router-link>
					</div>
				</div>
			</el-col>
		</el-col>
		<!--工具条-->
		<el-col :span="18" class="toolbar" style="background:#fff;">
			<el-pagination layout="prev, pager, next" @current-change="handleCurrentChange" :page-size="10" :total="total" style="float:right;">
			</el-pagination>
		</el-col>
	</section>
</template>

<script>
	import { state } from '../../vuex/state'
	import util from '../../common/js/util'
	import {baseUrl} from '../../api/api';

	export default {
		data() {
			return {
				FxsList:[],
				total: 0,
				page: 1,
				condition:'',
				value:'',
				states:[
					{
						value:'1',
						label:'昵称'
					},{
						value:'2',
						label:'真实姓名'
					},{
						value:'3',
						label:'手机号'
					}
				]
			}
		},
		methods: {
			getlist(){
				const _this = this
				const params = {
					pageNum:this.page,
					size:8,
					storeId:localStorage.getItem("storeId")
				}
				if(this.condition === '1'){
					params.nickName = this.value
				}else if(this.condition === '2'){
					params.realName = this.value
				}else if(this.condition === '3'){
					params.mobile = this.value
				}
				console.log(params)
				$.ajax({
	                type:'POST',
	                dataType:'json',
	                url:baseUrl+"/api/shopConfig/selectListBySeller",
	                data:JSON.stringify(params),
	                contentType:'application/json;charset=utf-8',
	                success:function(data){
	                  	console.log(data)
	                  	_this.total = data.data.total
	                  	_this.FxsList = data.data.list
	                }
	            });
			},
			handleCurrentChange(val) {
				this.page = val;
				this.getlist();
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
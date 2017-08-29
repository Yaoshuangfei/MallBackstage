<template>
	<section>

		<el-col :span="24" style="position: relative;background: #cab78c;height:48px;line-height: 48px;color: #fff;font-size: 16px;padding-left: 20px;margin-bottom: 20px;">
			我的分销商（{{FxsList.length}}）
		</el-col>

		<!--工具条-->
		<!--<el-col :span="24" style="padding-bottom: 0px;background: #fff">-->
			<!--<h3></h3>-->
		<!--</el-col>-->
		<el-col :span="24" style="padding-bottom: 0px;background: #fff">
			<el-col :xs="6" :sm="6" :md="6" :lg="6" v-for="item in FxsList" style='margin-bottom: 20px'>
				<div class="agen_div" style="height:260px;border: 1px solid #cab78c;">
					<div>
						<img style="width: 140px;height:140px;border-radius: 50%;" :src="item.coreUser.headImg" v-if="item.coreUser.headImg !== null">
						<img v-else style="width: 140px;height:140px;border-radius: 50%;" src="http://resources.51att.cn/ATTACHMENT/ATTACHMENT/1bccc3cf-8d44-4482-84e1-82d84d56e25c.png">
					</div>
					<div>{{item.coreUser.nickName}}</div>
					<div>{{item.shopRoleName}}</div>
					<div style="color: #9f3333;">
						<router-link :to="{ name: '查看下级', params: { id: item.id , name:item.coreUser.nickName}}" style="color: #9f3333;">
							<el-button  type="text" style="color: #9f3333;">查看下级</el-button>
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
				page: 1
			}
		},
		methods: {
			getlist(){
				const _this = this
				const params = {
					pageNum:this.page,
					size:8,
//					storeId:state.storeId
					storeId:localStorage.getItem("storeId")
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
		height: 260px;
		text-align: center;

	}
	.agen_div div:nth-child(1){
		margin-top: 20px;
	}
	.agen_div div:nth-child(2){
		text-align: center;
		margin-top: 20px;
		font-size: 18px;
		color: #616161;
		white-space:nowrap;
		text-overflow:ellipsis;
		-o-text-overflow:ellipsis;
		overflow: hidden;
	}
	.agen_div div:nth-child(3){
		text-align: center;
		font-size: 12px;
		color: #616161;
	}
	.agen_div div:nth-child(4){
		text-align: center;
	}
	.agen_div div:nth-child(4) a{
		color: #9f3333;
	}
</style>
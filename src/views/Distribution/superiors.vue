<template>
	<section>
		<!--工具条-->
		<el-col :span="24" style="padding-bottom: 0px;background: #fff">
			<h3>我的分销商（{{list.length}}）- 上级  - {{name}}</h3>
		</el-col>
		<el-col :span="24">
			<el-col :span="4" style="margin-top: 20%" class='bor_div_left'>{{name}}</el-col>
			<el-col :span="18" style="margin-left: 60px;margin-bottom: 10px; ">
				<el-col :span="24" style="margin-top: 10px" v-for="item in list">
					<el-col :span="4" class='bor_div'>
						<el-col :span="24">{{item.coreUser.nickName}}</el-col>
						<el-col :span="24">{{item.shopRoleName}}</el-col>
						<el-col :span="24">{{item.coreUser.mobile}}</el-col>
					</el-col>
					<el-col :span="10" style="margin-left: 60px">
						<el-col :span="24" v-for="items in item.list" style='margin-bottom: 10px;'>
							<el-col :span="24" class='bor_div'>
								<el-col :span="24">{{items.coreUser.nickName}}</el-col>
								<el-col :span="24">{{items.shopRoleName}}</el-col>
								<el-col :span="24">{{items.coreUser.mobile}}</el-col>
							</el-col>
						</el-col>
					</el-col>
				</el-col>
			</el-col>

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
				name:'',
				list:[]
			}
		},
		methods: {
			getlist(){
				const _this = this
				const params = {
					id:this.$route.params.id,
					lvl:3
				}
				$.ajax({
	                type:'POST',
	                dataType:'json',
	                url:baseUrl+"/api/shopConfig/selectChildLst",
	                data:JSON.stringify(params),
	                contentType:'application/json;charset=utf-8',
	                success:function(data){
	                	const info = data.data
	                  	console.log(data.data)
	                  	for(var i = 0;i<info.length;i++){
	                  		if(info[i].lvl === 0){
		                  		_this.name = info[i].coreUser.nickName
		                  	}
		                  	if(info[i].lvl === 1){
		                  		console.log(info[i])
		                  		console.log(info[i].id)
		                  		info[i].list = []
		                  		for(var x = 0;x<info.length;x++){
		                  			if(info[i].id === info[x].configId){
		                  				info[i].list.push(info[x])
		                  				console.log(info[x])
		                  			}
		                  		}
		                  		_this.list.push(info[i])
		                  	}
	                  	}
	                  	console.log(_this.list)
	                }
	            })
			}
		},
		mounted() {
			this.getlist();
		}
	}

</script>

<style>
	.bor_div_left{
		border: 1px solid #aaa;
		width:200px;
		height: 60px;
		line-height: 60px;
		text-align: center;
	}
	.bor_div{
		border: 1px solid #aaa;
		width:200px;
		height: 60px;
		/*line-height: 50px;*/
		text-align: center;
	}
</style>
<template>
	<section>
		<!--工具条-->
		<el-col :span="24" style="padding-bottom: 0px;background: #fff">
			<h3>我的分销商（{{list.length}}）- 上级  - {{name}}</h3>
		</el-col>
		<el-col :span="24" class="toolbar" style="padding-bottom: 0px;background: #fff">
			<el-form :inline="true">
				<el-form-item label="会员用户昵称">
					<el-input v-model="searchName"></el-input>
				</el-form-item>
				<el-form-item style="margin-right: inherit;">
					<el-button type="primary" v-on:click="getlist">查询</el-button>
				</el-form-item>
			</el-form>
		</el-col>
		<el-col :span="24" style="position: relative;">
			<!-- <el-col :span="20" style="position: absolute;z-index: 0;border-top:2px solid #cab78c;left:25px;top:80px;"></el-col> -->
			<!-- <el-col style="position: absolute;z-index: 0;border-left:2px solid #cab78c;left:225px;top:80px;width:1px;height:66%;"></el-col> -->
			<el-col :span="4" style="margin-top: 10px;" class='bor_div_left'>
				<el-col v-if="list.length !== 0" style="position: absolute;width:100%;border-top: 2px solid #cab78c;left:70px;bottom:87px;"></el-col>
				<img :src="headImg" style="width:160px;height:160px;border-radius: 50%;position: absolute;top:0;left:0;">
				<el-col :span="24" style="background: rgba(255,255,255,.8);width:160px;height:60px;position: absolute;z-index:1;bottom:-2px;left:0;">{{name}}</el-col>
				<!--<img style="width:160px;height:160px;border-radius: 50%;position: absolute;top:0;left:0;"-->
					 <!--:src="item.coreUser.headImg" v-if="item.coreUser.headImg !== null">-->
				<!--<img v-else-->
					 <!--style="width:160px;height:160px;border-radius: 50%;position: absolute;top:0;left:0;"-->
					 <!--src="http://resources.51att.cn/ATTACHMENT/ATTACHMENT/1bccc3cf-8d44-4482-84e1-82d84d56e25c.png">-->
				<!--<el-col :span="24"-->
						<!--style="background: rgba(255,255,255,.8);width:160px;height:60px;position: absolute;z-index:1;bottom:0;left:0;">-->

				<!--</el-col>-->

			</el-col>
			<el-col :span="15" style="margin-left: 160px;margin-bottom: 10px;">
				<el-col :span="24" style="margin-top: 10px; position: relative;" v-for="(item,index) in list">
					<el-col :span="4" class='bor_div'>
						<el-col style="position: absolute;width:100%;border-top: 2px solid #cab78c;left:-95px;bottom:87px;"></el-col>
						<el-col v-if="list.length-1 !== index" style="position: absolute;width:100%;border-left: 2px solid #cab78c;left:-95px;bottom:87px;height: 108%;top: 45%"></el-col>
						<el-col v-if="item.list.length !== 0" style="position: absolute;width:100%;border-top: 2px solid #cab78c;left:95px;bottom:87px;"></el-col>

						<div @click="onDown(item.id)" style="cursor: pointer;">
							<img style="width:160px;height:160px;border-radius: 50%;position: absolute;top:0;left:0;" :src="item.coreUser.headImg" v-if="item.coreUser.headImg !== null">
							<img v-else style="width:160px;height:160px;border-radius: 50%;position: absolute;top:0;left:0;" src="http://resources.51att.cn/ATTACHMENT/ATTACHMENT/1bccc3cf-8d44-4482-84e1-82d84d56e25c.png">
							<el-col :span="24" style="background: rgba(255,255,255,.8);width:160px;height:60px;position: absolute;z-index:1;bottom:-2px;left:0;">
							<el-col :span="24" v-if="item.coreUser !== null">{{item.coreUser.nickName}}</el-col>
							<el-col :span="24">{{item.shopRoleName}}</el-col>
							<el-col :span="24" v-if="item.coreUser !== null">{{item.coreUser.mobile}}</el-col>
							</el-col>
						</div>
					</el-col>
					<!--<el-col v-if="item.list.length > 0" :span="1" style="border:1px solid #ddd;margin-top: 20px;margin-left: 10px;"></el-col>-->
					<el-col :span="10" style="margin-left: 180px">
						<el-col :span="24" v-for="(items,indexs) in item.list">
							<el-col :span="24" class='bor_div'>
								<el-col style="position: absolute;width:100%;border-top: 2px solid #cab78c;left:-95px;bottom:87px;"></el-col>
								<el-col v-if="item.list.length-1 !== indexs" style="position: absolute;width:100%;border-left: 2px solid #cab78c;left:-95px;bottom:87px;height: 100%;top: 45%"></el-col>
								<div @click="onDown(items.id)" style="cursor: pointer;">
									<img style="width:160px;height:160px;border-radius: 50%;position: absolute;top:0;left:0;" :src="item.coreUser.headImg" v-if="item.coreUser.headImg !== null">
									<img v-else style="width:160px;height:160px;border-radius: 50%;position: absolute;top:0;left:0;" src="http://resources.51att.cn/ATTACHMENT/ATTACHMENT/1bccc3cf-8d44-4482-84e1-82d84d56e25c.png">
									<el-col :span="24" style="background: rgba(255,255,255,.8);width:160px;height:60px;position: absolute;z-index:1;bottom:-2px;left:0;">
										<el-col :span="24" v-if="items.coreUser !== null">{{items.coreUser.nickName}}</el-col>
										<el-col :span="24">{{items.shopRoleName}}</el-col>
										<el-col :span="24" v-if="items.coreUser !== null">{{items.coreUser.mobile}}</el-col>
									</el-col>

								</div>
							</el-col>
						</el-col>
					</el-col>
					<!--<el-col v-if="item.list.length > 0" :span="4" style="border-top:1px solid #bbb;margin-left: 234px;"></el-col>-->
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
                searchName:'',
                name:this.$route.params.name,
                list:[],
                id:this.$route.params.id,
                initParams:{},
				headImg:''
            }
        },
        methods: {
            onDown(val){
                console.log(val)
                this.id = val
                this.list = []
                this.getlist()
            },
            getlist(){
                const _this = this
                const params = {
                    id:this.id,
                    lvl:3,
                    searchName:this.searchName
                }
                // this.initParams = params
                $.ajax({
                    type:'POST',
                    dataType:'json',
                    url:baseUrl+"/api/shopConfig/selectChildLst",
                    data:JSON.stringify(params),
                    contentType:'application/json;charset=utf-8',
                    success:function(data){
                        const info = data.data
//                        console.log(info)
                        for(var i = 0;i<info.length;i++){

                            if(info[i].lvl === 0){
                                _this.name = info[i].coreUser.nickName;
                                if(info[i].coreUser.headImg == null){
                                    _this.headImg = 'http://resources.51att.cn/ATTACHMENT/ATTACHMENT/1bccc3cf-8d44-4482-84e1-82d84d56e25c.png'
								}else {
                                    _this.headImg = info[i].coreUser.headImg
								}
                            }
                            if(info[i].lvl === 1){
                                info[i].list = []
                                for(var x = 0;x<info.length;x++){
                                    if(info[i].id === info[x].configId){
                                        info[i].list.push(info[x])
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
		border-radius: 50%;
		width:160px;
		height: 160px;
		/*line-height: 60px;*/
		text-align: center;
		position: relative;
		/*overflow: hidden;*/
	}
	.bor_div{
		border: 1px solid #aaa;
		border-radius: 50%;
		width:160px;
		height: 160px;
		/*line-height: 60px;*/
		text-align: center;
		position: relative;
		/*overflow: hidden*/
	}
</style>
<template>
	<section>
		<el-col :span="24" style="position: relative;background: #cab78c;height:48px;line-height: 48px;color: #fff;font-size: 16px;padding-left: 20px;margin-bottom: 20px;">
			代理商转店铺发货配置
			<el-form :inline="true" style="position: absolute;top:6px;right:0;">
				<el-form-item>
					<el-button type="primary" v-on:click="addbanner" style="background: transparent;border: transparent;">保存</el-button>
				</el-form-item>
			</el-form>
		</el-col>
		<el-col :span="24" style="width:100%;margin-top: 20px;padding-left: 60px;">
			<el-col :span="6" style="margin-top: 8px">是否金豆抵扣</el-col>
			<el-radio-group v-model="radio">
				<el-radio   label="0" style="margin-right: 150px;">是</el-radio>
				<el-radio   label="1">否</el-radio>
			</el-radio-group>
		</el-col>
		<el-col :span="24" style="width:100%;margin-top: 20px;padding-left: 60px;">
			<el-col :span="6" style="margin-top: 8px">代理商分享转店铺是否分佣</el-col>
			<el-col :span="10">
				<el-radio-group v-model="radios" @change="sffy">
					<el-radio   label="0" style="margin-right: 150px;">是</el-radio>
					<el-radio   label="1">否</el-radio>
				</el-radio-group>
			</el-col>
		</el-col>
		<el-col :xs="14" :sm="14" :md="14" :lg="14" style="font-size: 16px;color:#616161;margin-bottom: 20px; margin-top: 80px;">购买身份价格</el-col>
		<el-col :span="24">
			<el-col :span="6" :offset="5"><el-button type="text" @click="addfyBtn">添加+</el-button><el-button type="text" @click="delfyBtn">删除-</el-button></el-col>
		</el-col>
		<el-col :span="24" id="adddd" style="" v-show="goodList">
			<el-table  :data="identity"  style="width: 30%;min-width: 300px;text-align: center;">
				<el-table-column type="index">
				</el-table-column>
				<el-table-column prop="name" label="分佣比">
				</el-table-column>
			</el-table>
			<!--<el-col :span="24" style="text-align: center;margin: 20px 0 ;">分佣21级数</el-col>-->
			<el-col v-for="item in fyObjList" :span="24"  style="text-align: center;">
				<el-col :span="2" style="height:40px;line-height: 40px;border-bottom: 1px solid #dfe6ec;border-left:1px solid #dfe6ec;border-right:1px solid #dfe6ec; " >{{item.id+1}}级</el-col>
				<el-col :span="15">
					<!--<el-col :span="10">-->
						<!--<el-input v-model="item.name" type="text" ></el-input>-->
					<!--</el-col>-->
					<el-col :span="9">
						<el-input v-model="item.value" type="text" style="border-bottom: 1px solid #dfe6ec;border-right:1px solid #dfe6ec;height:39px;"></el-input>
					</el-col>
				</el-col>
			</el-col>
			<el-col :span="8" style="text-align: center;margin-top: 30px;">
				<el-button type="primary" @click.native="baochun">确定</el-button>
			</el-col>
		</el-col>
		<el-col :span="24" style="text-align: justify;font-size: 12px;color: red;margin-top: 20px;">备注：分佣的比例为单价的百分之几，假设：商品为100元，设置分佣级数为一级且分佣比为5%，那么代理商获得5元的分享佣金！</el-col>

	</section>
</template>

<script>
	import { state } from '../../vuex/state'
	import util from '../../common/js/util'
	import {baseUrl , editUser, addUser } from '../../api/api';

	export default {
		data() {
			return {
                identity:[],
                fyObjList:[
					{value:'',id:0}
				],
                tabletest:[],
                goodList:true,
                radio:'0',
                radios:'0',
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
            sffy(){
				if(this.radios == 1){
                    this.goodList = false;
                    this.tabletest = '';
				}else {
                    this.goodList = true
				}
			},
            baochun(){
                this.tabletest = JSON.stringify(this.fyObjList)
                console.log(this.tabletest)
			},
            addfyBtn(){
                const obj = {}
                obj.id = this.fyObjList.length
                obj.value = ''
                this.fyObjList.push(obj)
            },
            delfyBtn(){
                const obj = {}
                obj.id = this.fyObjList.length -1
                obj.value = ''
                this.fyObjList.pop()
            },
            addbanner: function () {
                const _this = this
                const params = {
                    id:localStorage.getItem("storeId"),
                    agentIsGold:parseInt(this.radio),
                    agentComData:this.tabletest
                }
                console.log(params)
                $.ajax({
                    type:'POST',
                    dataType:'json',
                    url:baseUrl+"/api/store/updateAgent",
                    data:JSON.stringify(params),
                    contentType:'application/json;charset=utf-8',
                    success:function(data){
                        if(data.success == false){
                            alert(data.msg);
						}else {
                            console.log(data);
                            _this.getlist();
						}

                    }
                });
            },
			onSubmit(){
				const _this = this
				const params = {
					withdrawalsWeekStr:this.roleId.toString(),
					adminDiscount:parseInt(this.adminDiscount),
					minPrice:parseInt(this.minPrice),
                    storeId:localStorage.getItem("storeId")
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
                    storeId:localStorage.getItem("storeId"),
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
	                url:baseUrl+"/api/store/selectByUserId",
	                data:JSON.stringify({}),
	                contentType:'application/json;charset=utf-8',
	                success:function(data){
	                    console.log(data)
                        if (data.data.agentComData == '' || data.data.agentComData == null) {
                            if (data.data.agentIsGold == 0) {
                                _this.radio = '0'
                            } else {
                                _this.radio = '1'
                            }
                        }else{
                            const comData = eval('(' + data.data.agentComData + ')')
                            _this.fyObjList = comData;
                            if (data.data.agentIsGold == 0) {
                                _this.radio = '0'
                            } else {
                                _this.radio = '1'
                            }
						}

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
	#adddd .el-input .el-input__inner {text-align: center;border: transparent;width:90%;}
	#adddd .el-table__empty-block  {display: none;}
	/*.agen_div{*/
		/*border: 1px solid #aaa;*/
		/*width:200px;*/
		/*height: 300px;*/

	/*}*/
	/*.agen_div div:nth-child(1){*/
		/*border: 1px solid #aaa;*/
		/*width:100px;*/
		/*height: 100px;*/
		/*border-radius: 50px;*/
		/*margin-left: 45px;*/
		/*margin-top: 20px;*/
	/*}*/
	/*.agen_div div:nth-child(2){*/
		/*width: 200px;*/
		/*text-align: center;*/
		/*margin-top: 20px;*/
	/*}*/
	/*.agen_div div:nth-child(3){*/
		/*width: 200px;*/
		/*text-align: center;*/
		/*margin-top: 20px;*/
	/*}*/
	/*.agen_div div:nth-child(4){*/
		/*width: 200px;*/
		/*text-align: center;*/
		/*margin-top: 20px;*/
	/*}*/
</style>

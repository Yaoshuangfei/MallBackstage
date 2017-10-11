<template>
	<section>
		<!--工具条-->
		<el-col :span="24" class="toolbar" style="padding-bottom: 0px;background: #fff">
			<el-form :inline="true" :model="filters">
				<!-- <el-form-item>
					<el-input v-model="filters.name" placeholder="支付银行"></el-input>
				</el-form-item> -->
				<el-form-item label="订单来源">
					<el-select v-model="filters.source" clearable style="width:150px;">
						<el-option v-for="item in source" :label="item.label" :value="item.value">
						</el-option>
					</el-select>
				</el-form-item>
				<el-form-item label="状态">
					<el-select v-model="filters.status" clearable style="width:150px;">
				      <el-option v-for="item in states" :label="item.label" :value="item.value">
				      </el-option>
				    </el-select>
				</el-form-item>
				<el-form-item label="搜索类型">
				    <el-select v-model="filters.type" clearable style="width:150px;">
				      <el-option v-for="item in options" :label="item.label" :value="item.value">
				      </el-option>
				    </el-select>
				</el-form-item>
				<el-form-item>
				    <el-input v-model="filters.name"></el-input>
				</el-form-item>
				<el-form-item style="margin-right: inherit;">
					<el-button type="primary" v-on:click="getlist">查询</el-button>
					<el-button type="primary" @click="exportExcel">导出订单</el-button>
				</el-form-item>
			</el-form>
		</el-col>
		<el-col :span="24" style="background: #cab78c;color: #fff;font-size: 16px;height:48px;line-height: 48px;text-align: center;">
			<el-col style="width:340px;">商品详情</el-col>
			<el-col style="width:98px;">单价</el-col>
			<el-col style="width:90px;">数量</el-col>
			<el-col style="width:90px;">买家</el-col>
			<el-col style="width:100px;">订单总价</el-col>
			<el-col style="width:100px;">发货人</el-col>
			<el-col style="width:84px;">状态</el-col>
		</el-col> <!-- v-for="item in selectSubjectStatus" -->
		<el-col :span="24" class="table_div" v-for="item in selectSubjectStatus">
			<el-col :span="24"  class="table_div_head">
				<el-col :span="6">订单编号：{{item.id}}</el-col>
				<el-col :span="7">下单时间：{{item.createTime}}</el-col>
				<el-col :span="1" :offset="3">{{item.totalMoney}}</el-col>
				<el-col :span="3" :offset="1">{{item.consign.nickName}}</el-col>
				<el-col :span="2">
					<router-link :to="{ name: '订单详情', params: { id: item.id ,index: 0}}">
						<el-button style="margin-top:-5px;color: #9f3333;"  type="text">查看订单</el-button>
					</router-link>
				</el-col>
			</el-col>
			<el-col :span="24" v-for="items in item.orderGoods">
				<el-col :span="3">
					<img v-if="items.picture !== 'icon.png' " style="width: 100px;margin-left: 40px;margin-top: 30px" :src="items.picture">
					<img v-else style="width: 100px;margin-left: 40px;margin-top: 30px" src="http://resources.51att.cn/ATTACHMENT/ATTACHMENT/1bccc3cf-8d44-4482-84e1-82d84d56e25c.png">
				</el-col>
				<el-col style="width:180px;" :offset="1" class="describe_fiast">
				{{items.productName}}
				</el-col>
				<el-col style="text-align: center;width:98px;" class="describe">{{items.productPrice}}</el-col>
				<el-col style="text-align: center;width:90px;" class="describe">{{items.quantity}}</el-col>
				<el-col style="text-align: center;width:137px;" class="describe">{{item.consignee}}</el-col>
				<el-col style="text-align: center;width:84px;margin-left: 190px;" class="describe">{{items.orderStatus}}</el-col>
			</el-col>
		</el-col>
		<!--工具条-->
		<el-col :span="24" class="toolbar" style="background:#fff;">
			<el-pagination layout="prev, pager, next" @current-change="handleCurrentChange" :page-size="10" :total="total" style="float:right;">
			</el-pagination>
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
                typeId:'',
				radio: '0',
				checked: true,
				value:'',
				value1:'',
				value2:'',
				states:[{
					value:'',
					label:'所有订单'
				},{
					value:1,
					label:'支付中'
				},{
					value:2,
					label:'支付成功'
				},{
					value:3,
					label:'支付失败'
				},{
					value:4,
					label:'已取消'
				},{
					value:5,
					label:'卖家已发货'
				},{
					value:6,
					label:'已收货'
				},{
					value:7,
					label:'已评价'
				},{
					value:8,
					label:'交易完成'
				},{
					value:9,
					label:'售后处理'
				},{
					value:10,
					label:'已删除'
				}],
				selectSubjectStatus: [],
				options: [{
		          value: '1',
		          label: '订单编号'
		        }, {
		          value: '2',
		          label: '快递单号'
		        }],
		        source: [{
                    value: '0',
                    label: 'app'
                }, {
                    value: '1',
                    label: '微信'
                }],
				filters: {
					name: '',
					status:'',
					type:'',
                    source:''
				},
				users: [],
				total: 100,
				page: 1,
				listLoading: false,
				sels: [],//列表选中列

				editFormVisible: false,//编辑界面是否显示
				editLoading: false,
				editFormRules: {
					name: [
						{ required: true, message: '请输入姓名', trigger: 'blur' }
					]
				},
				//编辑界面数据
				editForm: {
					id: 0,
					username: '',
					sex: -1,
					age: 0,
					birth: '',
					addr: ''
				},

				addFormVisible: false,//新增界面是否显示
				addLoading: false,
				//新增界面数据
				orderDetails: {
				},
				orderInformation:[]
			}
		},
		methods: {
			getlist(){
				const _this = this
				const params = {
					pageNum:this.page,
					size:10,
//					storeId:state.storeId,
					storeId:localStorage.getItem("storeId"),
					orderStatus:this.filters.status,
					orderId:'',
					expno:'',
					source:this.filters.source
				}
				if(this.filters.type !== ''){
					if(this.filters.type === '1'){
						params.orderId = this.filters.name
					}else if(this.filters.type === '2'){
						params.expno = this.filters.name
					}
				}
				if(this.filters.num !== ''){
                    if(this.filters.num === '0'){
                        params.orderId = this.filters.name
                    }else if(this.filters.num === '1'){
                        params.expno = this.filters.name
                    }
                }
				// console.log(params)
				$.ajax({
                    type:'POST',
                    dataType:'json',
                    url:baseUrl+"/api/orderMall/selectListAll",
                    data:JSON.stringify(params),
                    contentType:'application/json;charset=utf-8',
                    error: function (XMLHttpRequest, textStatus, errorThrown) {},
                    success:function(data){
                    	const info = data.data
                    	 console.log(info)
                    	_this.total = info.total
                    	_this.selectSubjectStatus = info.list
                    	// console.log(_this.selectSubjectStatus)
                    	for(var i = 0;i<_this.selectSubjectStatus.length;i++){
                    		if(_this.selectSubjectStatus[i].createTime !== null){
		                		_this.selectSubjectStatus[i].createTime = new Date(_this.selectSubjectStatus[i].createTime).toLocaleString()
                    		}
		                	for(var x = 0;x<_this.selectSubjectStatus[i].orderGoods.length;x++){
		                		if(_this.selectSubjectStatus[i].orderGoods[x].orderStatus === 1) {
			                		_this.selectSubjectStatus[i].orderGoods[x].orderStatus = '支付中'
			                	}else if(_this.selectSubjectStatus[i].orderGoods[x].orderStatus === 2) {
			                		_this.selectSubjectStatus[i].orderGoods[x].orderStatus = '支付成功'
			                	}else if(_this.selectSubjectStatus[i].orderGoods[x].orderStatus === 3) {
			                		_this.selectSubjectStatus[i].orderGoods[x].orderStatus = '支付失败'
			                	}else if(_this.selectSubjectStatus[i].orderGoods[x].orderStatus === 4) {
			                		_this.selectSubjectStatus[i].orderGoods[x].orderStatus = '已取消'
			                	}else  if(_this.selectSubjectStatus[i].orderGoods[x].orderStatus === 5) {
			                		_this.selectSubjectStatus[i].orderGoods[x].orderStatus = '卖家已发货'
			                	}else  if(_this.selectSubjectStatus[i].orderGoods[x].orderStatus === 6) {
			                		_this.selectSubjectStatus[i].orderGoods[x].orderStatus = '已收货'
			                	}else  if(_this.selectSubjectStatus[i].orderGoods[x].orderStatus === 7) {
			                		_this.selectSubjectStatus[i].orderGoods[x].orderStatus = '已评价'
			                	}else  if(_this.selectSubjectStatus[i].orderGoods[x].orderStatus === 8) {
			                		_this.selectSubjectStatus[i].orderGoods[x].orderStatus = '交易完成'
			                	}else  if(_this.selectSubjectStatus[i].orderGoods[x].orderStatus === 9) {
			                		_this.selectSubjectStatus[i].orderGoods[x].orderStatus = '售后处理'
			                	}else  if(_this.selectSubjectStatus[i].orderGoods[x].orderStatus === 10) {
			                		_this.selectSubjectStatus[i].orderGoods[x].orderStatus = '已删除'
			                	}
		                	}
		                }
		                // console.log(_this.selectSubjectStatus)
                    }
                });
			},
			handleCurrentChange(val) {
				this.page = val;
				this.getlist();
			},
			exportExcel(){
				const params = {
					orderType:'5,6,8',
					orderStatus:'2,5,6,7,8',
					startTime:'',
					endTime:''
				}
				$.ajax({
                    type:'POST',
                    dataType:'json',
                    url:baseUrl+"/api/storeInformation/order",
                    data:JSON.stringify(params),
                    contentType:'application/json;charset=utf-8',
                    success:function(data){
                        console.log(data)
                        window.location.href = data.msg
                    }
                })
			}
		},
		mounted() {
			if(this.$route.params.id !== ':id' ){
			    this.filters.status = this.$route.params.id
                this.getlist();
			}else{
                this.getlist();
			}
		}
	}

</script>

<style>
	.table_div{
		margin-top: 20px;
		border: 1px solid #e6eef9 ;
		padding-bottom: 20px;
	}
	.table_div_head{
		padding-top:15px;
		padding-left: 40px;
		height:50px;
		border-bottom: 1px dashed #cab78c;
		/*border-right: 1px solid #aaa;*/
	}
	.img_shangp{
		width:100px;
		height: 100px;
		border: 1px solid #aaa;
		margin-left: 40px;
		margin-top: 20px;
	}
	.describe_fiast{
		margin-top:40px;
	}
	.describe{
		margin-top: 60px;
	}
</style>
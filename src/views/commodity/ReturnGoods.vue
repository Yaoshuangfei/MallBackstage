<template>
	<section>
		<!--工具条-->
		<el-col :span="24" class="toolbar" style="padding-bottom: 0px;background: #fff">
			<el-form :inline="true" :model="filters">
				<el-form-item label="商品名称">
				     <el-input v-model="filters.name"></el-input>     
				</el-form-item>
				<el-form-item>
					<el-button type="primary" v-on:click="getlist">查询</el-button>
				</el-form-item>
			</el-form>
		</el-col>

		<!--列表-->
		<el-col :span="24">
			<el-col :span="6" :offset="4">商品名称</el-col>
			<el-col :span="2">价格</el-col>
			<el-col :span="2">库存</el-col>
			<el-col :span="5">退款原因</el-col>
			<!-- <el-col :span="2" style="margin-left: 20px">订单总价</el-col>
			<el-col :span="2">状态</el-col> -->
			<el-col :span="5">操作</el-col>	
		</el-col> <!-- v-for="item in selectSubjectStatus" -->
		<el-col :span="24" class="table_div" v-for="item in table">
			<el-col :span="24"  class="table_div_head">
				<el-col :span="6">订单编号：{{item.orderId}}</el-col>
				<!-- <el-col :span="4">下单时间：2017-08-09 12:20</el-col> -->
			</el-col>
			<el-col :span="24">
				<el-col :span="3" :offset="1" style="margin-top: 20px;">
					<img style="width: 100px;" :src="item.orderGoods.picture">
				</el-col>
				<el-col :span="6" class="describe">
				{{item.orderGoods.productName}}
				</el-col>
				<el-col :span="2" class="describe">{{item.refundPrice}}</el-col>
				<el-col :span="2" class="describe">{{item.storage}}</el-col>
				<el-col :span="5" class="describe" v-if="item.causeType === '1' ">无理由退款</el-col>
				<el-col :span="5" class="describe" v-if="item.causeType === '2' ">商品质量问题</el-col>
				<el-col :span="5" class="describe" v-if="item.causeType === '3' ">收到商品与描述不符</el-col>
				<el-col :span="5" class="describe" v-if="item.causeType === '4' ">商品错发、漏发</el-col>
				<el-col :span="4" class="describe">
					<el-button type="text" v-on:click="seeSubmit(item)">查看</el-button>
					<el-button v-if="item.refundStatus === 1" type="text" v-on:click="showok(item)">同意</el-button>
					<el-button v-if="item.refundStatus === 1" type="text" v-on:click="showno(item)">不同意</el-button>
				</el-col>
			</el-col>
		</el-col>
		<!--工具条-->
		<el-col :span="18" class="toolbar" style="background:#fff;">
			<!-- <el-button type="danger" @click="batchRemove" :disabled="this.sels.length===0">批量删除</el-button> --> 
			<el-pagination layout="prev, pager, next" @current-change="handleCurrentChange" :page-size="10" :total="total" style="float:right;">
			</el-pagination>
		</el-col>
		<!-- 同意退款 -->
		<el-dialog title="同意退款" v-model="tongyi" :close-on-click-modal="false" >
			<el-form label-width="100px">
				<el-form-item label="密码">
					<el-input v-model="typasswd" type="password" auto-complete="off"></el-input>
				</el-form-item>
				<el-form-item v-if="refundType === '2'" label="退货地址">
					<el-input v-model="tyly" type="text" auto-complete="off"></el-input>
				</el-form-item>
				<el-form-item v-else label="同意原因">
					<el-input v-model="tyly" type="text" auto-complete="off"></el-input>
				</el-form-item>
			</el-form>
			<div slot="footer" class="dialog-footer" style="text-align: center;">
				<el-button type="primary" @click.native="okSubmit" :loading="editLoading">保存</el-button>
				<el-button type="primary" @click.native="tongyi = false">关闭</el-button>
			</div>
		</el-dialog>
		<!-- 不同意退款 -->
		<el-dialog title="同意退款" v-model="notongyi" :close-on-click-modal="false" >
			<el-form label-width="100px">
				<el-form-item label="密码">
					<el-input v-model="btypasswd" type="password" auto-complete="off"></el-input>
				</el-form-item>
				<el-form-item label="不同意原因">
					<el-input v-model="btyly" type="text" auto-complete="off"></el-input>
				</el-form-item>
			</el-form>
			<div slot="footer" class="dialog-footer" style="text-align: center;">
				<el-button type="primary" @click.native="noSubmit" :loading="editLoading">保存</el-button>
				<el-button type="primary" @click.native="notongyi = false">关闭</el-button>
			</div>
		</el-dialog>
		<!--编辑界面-->
		<el-dialog title="订单详情" v-model="editFormVisible" :close-on-click-modal="false" >
			<el-form :model="orderDetails" label-width="100px" :rules="editFormRules" ref="editForm">
				<el-form-item label="订单号">
					<div>{{orderDetails.orderNumber }}</div>
				</el-form-item>
				<el-form-item label="商品名称">
					<div>{{orderDetails.commodityName}}</div>
				</el-form-item>
				<el-form-item label="用户名">
					<div>{{orderDetails.userName }}</div>
				</el-form-item>
				<el-form-item label="实付金额">
					<div>{{orderDetails.amountPaid }}</div>
				</el-form-item>
				<el-form-item label="订单总价">
					<div>{{orderDetails.orderTotal }}</div>
				</el-form-item>
				<el-form-item label="订单状态">
					<div>{{orderDetails.orderStatus }}</div>
				</el-form-item>
				<el-form-item label="支付方式">
					<div>{{orderDetails.paymentMethod }}</div>
				</el-form-item>
				<el-form-item label="创建时间">
					<div>{{orderDetails.creationTime}}</div>
				</el-form-item>
				<el-form-item label="发货时间">
					<div>{{orderDetails.deliveryTime}}</div>
				</el-form-item>
				<el-col :span='24'></el-col>
			</el-form>
			<div slot="footer" class="dialog-footer" style="text-align: center;">
				<!-- <el-button type="primary" @click.native="editSubmit" :loading="editLoading">保存</el-button> -->
				<el-button type="primary" @click.native="editFormVisible = false">关闭</el-button>
			</div>
		</el-dialog>
	</section>
</template>

<script>
	import { state } from '../../vuex/state'
	import util from '../../common/js/util'
	import {baseUrl , editUser, addUser } from '../../api/api';

	export default {
		data() {
			return {
				tongyi: false,
				notongyi:false,
				btypasswd:'',
				btyly:'',
				typasswd:'',
				tyly:'',
				tyId:'',
				btyId:'',
				refundType:'',
				checked: true,
				value:'',
				value1:'',
				value2:'',
				selectSubjectStatus: [
				{
					value:'0',
					label:'所有订单'
				},{
					value:'1',
					label:'待付款'
				},{
					value:'2',
					label:'待发货'
				},{
					value:'3',
					label:'已发货'
				},{
					value:'4',
					label:'待评价'
				},{
					value:'5',
					label:'交易完成'
				},{
					value:'6',
					label:'退货'
				}],
				table:[],
				options: [{
		          value: '1',
		          label: '订单编号'
		        }, {
		          value: '2',
		          label: '快递单号'
		        }],
				filters: {
					name: '',
					status:'',
					type:''
				},
				users: [],
				total: 0,
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
				orderDetails: {},
				orderInformation:[]
			}
		},
		methods: {
			//性别显示转换
			seeSubmit(row){
				this.orderDetails = row
				this.editFormVisible = true
				console.log(row)
			},
			showok(row){
				this.tongyi = true
				this.tyId = row.refundId
				this.refundType = row.refundType
			},
			okSubmit(){
				const _this = this
				const params = {
					refundId:this.tyId,
					refundStatus:2,
					payPwd:this.typasswd,
					sellerCause:this.tyly
				}
				console.log(params)
				$.ajax({
	                type:'POST',
	                dataType:'json',
	                url:baseUrl+"/api/refundOrder/update",
	                data:JSON.stringify(params),
	                contentType:'application/json;charset=utf-8',
	                success:function(data){
	                  	console.log(data)
	                  	if(data.code === 1){
	                  		_this.tongyi = false
	                  	}
	                }
	            });
			},
			showno(row){
				this.notongyi = true
				this.btyId = row.refundId
			},
			noSubmit(){
				const _this = this
				const params = {
					refundId:this.btyId,
					refundStatus:3,
					payPwd:this.btypasswd,
					sellerCause:this.btyly
				}
				console.log(params)
				$.ajax({
	                type:'POST',
	                dataType:'json',
	                url:baseUrl+"/api/refundOrder/update",
	                data:JSON.stringify(params),
	                contentType:'application/json;charset=utf-8',
	                success:function(data){
	                  	console.log(data)
	                  	if(data.code === 1){
	                  		_this.notongyi = false
	                  	}
	                }
	            });
			},
			getlist(){
				const _this = this
				_this.table = []
				const params = {
					pageNum:this.page,
					size:10,
					storeId:localStorage.getItem("storeId")
				}
				$.ajax({
	                type:'POST',
	                dataType:'json',
	                url:baseUrl+"/api/refundOrder/selectAllList",
	                data:JSON.stringify(params),
	                contentType:'application/json;charset=utf-8',
	                success:function(data){
	                  	console.log(data)
	                  	const info = data.data
	                  	_this.table = info.list
	                  	_this.total = data.data.total
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
			// api/refundOrder/selectAllList
			// api/refundOrder/update
		}
	}

</script>

<style>
	
</style>
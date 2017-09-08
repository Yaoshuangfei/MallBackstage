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
			<el-col :span="4" :offset="4">商品详情</el-col>
			<el-col :span="2">单价</el-col>
			<el-col :span="2">数量</el-col>
			<el-col :span="2">买家</el-col>
			<el-col :span="2" style="margin-left: 20px">订单总价</el-col>
			<el-col :span="2">状态</el-col>
			<el-col :span="2">操作</el-col>
		</el-col> <!-- v-for="item in selectSubjectStatus" -->
		<el-col :span="24" class="table_div" v-for="item in table">
			<el-col :span="24"  class="table_div_head">
				<el-col :span="6">订单编号：111111111111111111</el-col>
				<el-col :span="4">下单时间：2017-08-09 12:20</el-col>
			</el-col>
			<el-col :span="24">
				<el-col :span="6" class="img_shangp">
				</el-col>
				<el-col :span="6" :offset="1" class="describe_fiast">
				你说神农级手机爱好的撒等哈收到哦啊是的哈是的哈
				</el-col>
				<el-col :offset="1" :span="3" class="describe">321</el-col>
				<el-col :span="2" class="describe">321</el-col>
				<el-col :span="3" class="describe">18767478564</el-col>
				<el-col :span="2" :offset="1"  class="describe">321</el-col>
				<el-col :span="1" class="describe">321</el-col>
			</el-col>
		</el-col>
		<!--工具条-->
		<el-col :span="18" class="toolbar" style="background:#fff;">
			<!-- <el-button type="danger" @click="batchRemove" :disabled="this.sels.length===0">批量删除</el-button> -->
			<el-pagination layout="prev, pager, next" @current-change="handleCurrentChange" :page-size="10" :total="total" style="float:right;">
			</el-pagination>
		</el-col>

		<!--编辑界面-->
		<el-dialog title="订单详情" v-model="editFormVisible" :close-on-click-modal="false" >
			<el-form :model="orderDetails" label-width="160px" :rules="editFormRules" ref="editForm">
				<el-form-item label="订单号">
					<div>{{orderDetails.orderNumber }}</div>
					<!-- <el-input v-model="addForm.name" type="text" auto-complete="off"></el-input> -->
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
				radio: '0',
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
				orderDetails: {
				},
				orderInformation:[]
			}
		},
		methods: {
			//性别显示转换
			formatSex: function (row, column) {
				return row.sex == 1 ? '男' : row.sex == 0 ? '女' : '未知';
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
	                  	// _this.total = data.data.total
	                  	// _this.table = data.data.list
	                }
	            });
			},
			handleCurrentChange(val) {
				this.page = val;
				this.getlist();
			},
			//显示编辑界面
			seeBtn: function (index, row) {
				this.editFormVisible = true;
				this.orderDetails = Object.assign({}, row);
			},
			//显示新增界面
			handleAdd: function () {
				this.addFormVisible = true;
				this.addForm = {
					name: '',
					sex: -1,
					age: 0,
					birth: '',
					addr: ''
				};
			},
			selsChange: function (sels) {
				this.sels = sels;
			},
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
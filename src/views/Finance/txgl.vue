<template>
	<section>
		<!--工具条-->
		<el-col :span="24" class="toolbar" style="padding-bottom: 0px;background: #fff">
			<el-form :inline="true" :model="filters">
				<el-form-item label="状态">
					<el-select v-model="filters.status" clearable>
				      <el-option v-for="item in selectSubjectStatus" :label="item.label" :value="item.value">
				      </el-option>
				    </el-select>
				</el-form-item>
				<el-form-item label="搜索类型">
				    <el-select v-model="filters.type" clearable>
				      <el-option v-for="item in options" :label="item.label" :value="item.value">
				      </el-option>
				    </el-select>
				</el-form-item>
				<el-form-item>
				    <el-input v-model="filters.name"></el-input>
				</el-form-item>
				<el-form-item>
					<el-button type="primary" @click="getlist">查询</el-button>
					<el-button type="primary">导出</el-button>
				</el-form-item>
			</el-form>
		</el-col>

		<!--列表-->
		<el-table :data="orderInformation" highlight-current-row v-loading="listLoading" style="width: 100%;min-width: 1080px;">
			<el-table-column prop="tradeNo" label="订单号">
			</el-table-column>
			<el-table-column prop="userName" label="用户名">
			</el-table-column>
			<el-table-column prop="mobile" label="手机号">
			</el-table-column>
			<el-table-column prop="quota" label="提现金额">
			</el-table-column>
			<el-table-column prop="payType" :formatter='formatterType' label="订单状态">
			</el-table-column>
			<el-table-column prop="centType" :formatter='formatterTime' label="提现时间">
			</el-table-column>
			<el-table-column label="操作">
				<template scope="scope">
					<!-- <el-button v-if='scope.row.index === 1' type='text' size="small" @click="handleEdit(scope.$index, scope.row)">暂停</el-button> -->
					<!-- <el-button v-else-if='scope.row.index === 0' :disabled="true" type='text' size="small" @click="handleEdit(scope.$index, scope.row)">已处理</el-button> -->
					<el-button type="text" size="small" @click="seeBtn(scope.$index, scope.row)">查看</el-button>
					<el-button type="text" size="small" @click="handleEdit(scope.$index, scope.row)">打款</el-button>
					<el-button type="text" size="small" @click="handleEdit(scope.$index, scope.row)">不通过</el-button>
				</template>
			</el-table-column>
		</el-table>

		<!--工具条-->
		<el-col :span="24" class="toolbar" style="background:#fff;">
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
				<el-form-item label="分佣详情">
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
					label:'全部'
				},{
					value:'1',
					label:'交易成功'
				},{
					value:'2',
					label:'交易失败'
				},{
					value:'3',
					label:'审核中付'
				}],
				options: [{
		          value: '1',
		          label: '订单编号'
		        }, {
		          value: '2',
		          label: '手机号'
		        }, {
		          value: '3',
		          label: '姓名'
		        }],
				filters: {
					name: '',
					status:'',
					type:''
				},
				users: [],
				total: 1,
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
					pageNum:_this.page,
					pageSize:10,
					payType:this.filters.status,
					tradeNo:'',
					userName:'',
					mobile:'',
					sort:4
				}
				if(this.filters.type === '1'){
					params.tradeNo = this.filters.name
				}else if(this.filters.type === '2'){
					params.userName = this.filters.name
				}else if(this.filters.type === '3'){
					params.mobile = this.filters.name
				}
				console.log(params)
				$.ajax({
                    type:'POST',
                    dataType:'json',
                    // url:"http://192.168.0.115:8080/api/store/userCashFlow/selectFlowList",
                    url:baseUrl+"/api/store/userCashFlow/selectFlowList",
                    data:JSON.stringify(params),
                    contentType:'application/json;charset=utf-8',
                    error: function (XMLHttpRequest, textStatus, errorThrown) {},
                    success:function(data){
                    	const info = data.data
                    	_this.orderInformation = info.list
                    	_this.total = info.total
                    }
                });
			},
			handleCurrentChange(val) {
				this.page = val;
				this.getlist();
			},
			formatterTime(row, column){
				let curTime = row.createTime;
                curTime = new Date(curTime).toLocaleString()
                return curTime
			},
			//支付方式
			formatterType(row, column) {
				let type = ''
				if(row.payType === '0'){
					type = '微信支付'
				}else if(row.payType === '1'){
					type = '支付宝支付'
				}else if(row.payType === '2'){
					type = '银联支付'
				}else if(row.payType === '3'){
					type = '余额支付'
				}else if(row.payType === '4'){
					type = '余额金豆混合支付'
				}else if(row.payType === '5'){
					type = '金豆支付'
				}
				return type
			},
			formatter(row, column){
				let type = ''
				if(row.centType === 1){
					type = '购买平台身份'
				}else if(row.centType === 2){
					type = '购买店铺身份'
				}else if(row.centType === 3){
					type = '购买产品'
				}else if(row.centType === 4){
					type = '补货 '
				}else if(row.centType === 5){
					type = '业务充值'
				}else if(row.centType === 6){
					type = '扫码支付'
				}
				return type
			}
		},
		mounted() {
			this.getlist();
		}
	}

</script>

<style>
	
</style>
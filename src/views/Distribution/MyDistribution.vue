<template>
	<section>
		<el-col :xs="14" :sm="14" :md="14" :lg="14" style="margin-top: 20px">
			<el-button type="primary" v-on:click="getUsers">添加</el-button>
		</el-col>
		<el-col :xs="14" :sm="14" :md="14" :lg="14" style="margin-top: 20px;margin-bottom: 20px">店铺等级</el-col>
		<el-col :xs="14" :sm="14" :md="14" :lg="14" style="margin-top: 20px;margin-bottom: 20px">
			<ul class="Grade">
				<li>联创</li>
				<li>联创</li>
				<li>联创</li>
			</ul>
		</el-col>
		<el-col :xs="24" :sm="24" :md="24" :lg="24" style="margin-bottom: 20px;margin-left: 50px">填写可购买的最高级身份，最多填写5个</el-col>
		<el-col :xs="14" :sm="14" :md="14" :lg="14" style="margin-bottom: 20px">规则</el-col>
		<el-col :xs="14" :sm="14" :md="14" :lg="14" style="margin-bottom: 20px">
			<el-col :xs="4" :sm="4" :md="4" :lg="4" style="margin-bottom: 20px;margin-left: 60px">身份是否升级</el-col>
			<el-col :xs="14" :sm="14" :md="14" :lg="14" style="margin-bottom: 20px">
				<el-select v-model="value" placeholder="请选择">
				    <el-option
				      v-for="item in options"
				      :key="item.value"
				      :label="item.label"
				      :value="item.value">
				    </el-option>
			  </el-select>
			</el-col>
		</el-col>
		<el-col :xs="24" :sm="24" :md="24" :lg="24" style="margin-bottom: 20px">
			<el-col :xs="3" :sm="3" :md="3" :lg="3" style="margin-bottom: 20px">邀请人身份</el-col>
			<el-col :xs="3" :sm="3" :md="3" :lg="3" style="margin-bottom: 20px">
				<el-select v-model="value1" placeholder="请选择">
				    <el-option
				      v-for="item in options1"
				      :key="item.value"
				      :label="item.label"
				      :value="item.value">
				    </el-option>
			  </el-select>
			</el-col>
		</el-col>
		<el-col :xs="24" :sm="24" :md="24" :lg="24" style="margin-bottom: 20px">
			<el-col :xs="3" :sm="3" :md="3" :lg="3" style="margin-bottom: 20px">被邀请人身份</el-col>
			<el-col :xs="3" :sm="3" :md="3" :lg="3" style="margin-bottom: 20px">
				<el-select v-model="value1" placeholder="请选择">
				    <el-option
				      v-for="item in options2"
				      :key="item.value"
				      :label="item.label"
				      :value="item.value">
				    </el-option>
			  </el-select>
			</el-col>
		</el-col>
		<!-- <el-col :xs="24" :sm="24" :md="24" :lg="24" style="margin-bottom: 20px;border: 1px solid #ddd;width:1000px;height: 400px;">
			<el-col :xs="24" :sm="24" :md="24" :lg="24" style="">
				<ul class="headerfx">
					<li>联创</li>
					<li>总代</li>
					<li>普通</li>
					<li>普通1</li>
					<li>普通2</li>
				</ul>
			</el-col>	
			<el-col :xs="24" :sm="24" :md="24" :lg="24" style="">
				<el-col :xs="4" :sm="4" :md="4" :lg="4" style="">
					<ul class="leftul">
						<li>自己拿</li>
						<li>一级</li>
						<li>二级</li>
						<li>三级</li>
						<li>四级</li>
						<li>五级</li>
					</ul>
				</el-col>
				<el-col :xs="14" :sm="14" :md="14" :lg="14" style="">
					
				</el-col>
			</el-col>
		</el-col> -->
		<!--列表-->
		
		<el-table :data="orderInformation" highlight-current-row v-loading="listLoading" style="width: 100%;min-width: 1080px;">
			<el-table-column prop="courierNumber" label="">
			</el-table-column>
			<el-table-column prop="userName" v-if="biaoti[0] " :label="biaoti[0]">
			</el-table-column>
			<el-table-column prop="amountPaid" v-if="biaoti[1] " :label="biaoti[1]">
			</el-table-column>
			<el-table-column prop="orderTotal" v-if="biaoti[2] " :label="biaoti[2]">
			</el-table-column>
			<el-table-column prop="orderStatus" v-if="biaoti[3] " :label="biaoti[3]">
			</el-table-column>
			<el-table-column prop="paymentMethod" v-if="biaoti[4] " :label="biaoti[4]">
			</el-table-column>
			<el-table-column label="操作">
				<template scope="scope">
					<el-button type="text" size="small" @click="seeBtn(scope.$index, scope.row)">修改</el-button>
					<el-button type="text" size="small" @click="handleEdit(scope.$index, scope.row)">删除</el-button>
				</template>
			</el-table-column>
		</el-table>
		<el-col :xs="1" :sm="1" :md="1" :lg="1" :offset="23" style="">
			<el-button type="primary" @click="seeBtn(scope.$index, scope.row)">增加</el-button>
		</el-col>
		
		<!--工具条-->
		<!-- <el-col :span="24" class="toolbar" style="background:#fff;"> -->
			<!-- <el-button type="danger" @click="batchRemove" :disabled="this.sels.length===0">批量删除</el-button> -->
			<!-- <el-pagination layout="prev, pager, next" @current-change="handleCurrentChange" :page-size="10" :total="total" style="float:right;"> -->
			<!-- </el-pagination> -->
		<!-- </el-col> -->
		<!--编辑界面-->
		<el-dialog title="修改" v-model="editFormVisible" :close-on-click-modal="false" >
			<el-form :model="orderDetails" label-width="60px" :rules="editFormRules" ref="editForm">
				<el-form-item label="">
					<div>{{orderDetails.courierNumber}}</div>
				</el-form-item>
				<el-form-item label="联创">
					<el-input v-model="orderDetails.userName" type="text" auto-complete="off"></el-input>
				</el-form-item>
				<el-form-item label="总代">
					<el-input v-model="orderDetails.amountPaid" type="text" auto-complete="off"></el-input>
				</el-form-item>
				<el-form-item label="普通">
					<el-input v-model="orderDetails.orderTotal" type="text" auto-complete="off"></el-input>
				</el-form-item>
				<el-form-item label="普通1">
					<el-input v-model="orderDetails.orderStatus" type="text" auto-complete="off"></el-input>
				</el-form-item>
				<el-form-item label="普通2">
					<el-input v-model="orderDetails.paymentMethod" type="text" auto-complete="off"></el-input>
				</el-form-item>
				<el-col :span='24'></el-col>
			</el-form>
			<div slot="footer" class="dialog-footer" style="text-align: center;">
				<el-button type="primary" @click.native="editSubmit" :loading="editLoading">确定</el-button>
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
				biaoti:['联创','总代'],
				options1: [{
		          value: '0',
		          label: '经理'
		        }, {
		          value: '1',
		          label: '总监'
		        },{
		          value: '2',
		          label: '联创'
		        },{
		          value: '3',
		          label: '总代'
		        },{
		          value: '4',
		          label: '普通'
		        }],
		        options2: [{
		          value: '0',
		          label: '联创'
		        }, {
		          value: '1',
		          label: '总代'
		        },{
		          value: '2',
		          label: '普通'
		        },{
		          value: '3',
		          label: '普通1'
		        },{
		          value: '4',
		          label: '普通2'
		        }],
				filters: {
					name: '',
					status:'',
					type:''
				},
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
					label:'退货'
				}],
				options: [{
		          value: '0',
		          label: '否'
		        }, {
		          value: '1',
		          label: '是'
		        },],
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
				orderInformation:[{
					courierNumber :'自己拿',
					userName:'200/5%',
					amountPaid :'200/5%',
					orderTotal :'',
					orderStatus :'',
					paymentMethod :''
				},{
					courierNumber :'一级',
					userName:'200/5%',
					amountPaid :'200/5%',
					orderTotal :'',
					orderStatus :'',
					paymentMethod :''
				},{
					courierNumber :'二级',
					userName:'200/5%',
					amountPaid :'200/5%',
					orderTotal :'',
					orderStatus :'',
					paymentMethod :''
				},{
					courierNumber :'三级',
					userName:'200/5%',
					amountPaid :'200/5%',
					orderTotal :'',
					orderStatus :'',
					paymentMethod :''
				}]
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
					accountId:'1',
					accessToken:'',
					resourceType:'',
					page:{
						pageNum:_this.page,
						pageSize:'10'
					}
				}
				console.log(params)
				$.post(baseUrl+"/admin/banner/getBannerByPage",
	             { param: JSON.stringify(params) },
	             function(data){
	             	const info = eval('(' + data + ')');
	                const response = JSON.parse(info);
	                const list = response.obj.results
	                console.log(response)
	                // _this.page = response.obj.total
	                _this.total = response.obj.totalRecord
	                for(var i = 0;i<list.length;i++){
	                	_this.table.push(list[i])
	                }
	              }
	         	)
			},
			handleCurrentChange(val) {
				this.page = val;
				this.getUsers();
			},
			//获取用户列表
			getUsers() {
				let para = {
					page: this.page,
					name: this.filters.name
				};
				this.listLoading = true;
				//NProgress.start();
				getUserListPage(para).then((res) => {
					this.total = res.data.total;
					this.users = res.data.users;
					this.listLoading = false;
					//NProgress.done();
				});
			},
			//删除
			handleDel: function (index, row) {
				this.$confirm('确认删除该记录吗?', '提示', {
					type: 'warning'
				}).then(() => {
					this.listLoading = true;
					//NProgress.start();
					let para = { id: row.id };
					removeUser(para).then((res) => {
						this.listLoading = false;
						//NProgress.done();
						this.$message({
							message: '删除成功',
							type: 'success'
						});
						this.getUsers();
					});
				}).catch(() => {

				});
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
			//编辑
			editSubmit: function () {
				
			},
			//新增
			addSubmit: function () {
				this.$refs.addForm.validate((valid) => {
					if (valid) {
						this.$confirm('确认提交吗？', '提示', {}).then(() => {
							this.addLoading = true;
							//NProgress.start();
							let para = Object.assign({}, this.addForm);
							para.birth = (!para.birth || para.birth == '') ? '' : util.formatDate.format(new Date(para.birth), 'yyyy-MM-dd');
							addUser(para).then((res) => {
								this.addLoading = false;
								//NProgress.done();
								this.$message({
									message: '提交成功',
									type: 'success'
								});
								this.$refs['addForm'].resetFields();
								this.addFormVisible = false;
								this.getUsers();
							});
						});
					}
				});
			},
			selsChange: function (sels) {
				this.sels = sels;
			},
			//批量删除
			batchRemove: function () {
				var ids = this.sels.map(item => item.id).toString();
				this.$confirm('确认删除选中记录吗？', '提示', {
					type: 'warning'
				}).then(() => {
					this.listLoading = true;
					//NProgress.start();
					let para = { ids: ids };
					batchRemoveUser(para).then((res) => {
						this.listLoading = false;
						//NProgress.done();
						this.$message({
							message: '删除成功',
							type: 'success'
						});
						this.getUsers();
					});
				}).catch(() => {

				});
			}
		},
		mounted() {
			// this.getlist();
		}
	}

</script>

<style>
	
	.Grade {
		list-style: none;
	}
	.Grade li{
		float: left;
		margin-right: 20px;
		width:200px;
		height: 50px;
		border: 1px solid #ddd;
		text-align: center;
		line-height: 50px;
	}
	.headerfx{
		list-style:none;
	}
	.headerfx li{
		float: left;
		margin-left: 150px
	}
	.leftul{
		list-style:none;
	}
	.leftul li{
		margin-bottom: 40px
	}
</style>
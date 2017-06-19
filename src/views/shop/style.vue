<template>
	<section>
		<el-row :gutter="10" style="margin-top: 40px">
			<el-col :xs="15" :sm="15" :md="15" :lg="15">
				<el-col :xs="24" :sm="24" :md="24" :lg="24">
					<el-col :xs="24" :sm="24" :md="24" :lg="24">模块一</el-col>
					<el-col :xs="8" :sm="8" :md="8" :lg="8">
						<div class="shopstyle"></div>
						<div style="margin-left: 60px">
							<el-button type="text" size="small" @click="seeBtn(scope.$index, scope.row)">使用</el-button>
							<el-button style="margin-left: 40px" type="text" size="small" @click="seeBtn(scope.$index, scope.row)">预览</el-button>
						</div>
					</el-col>
					<el-col :xs="8" :sm="8" :md="8" :lg="8">
						<div class="shopstyle"></div>
						<div style="margin-left: 60px">
							<el-button type="text" size="small" @click="seeBtn(scope.$index, scope.row)">使用</el-button>
							<el-button style="margin-left: 40px" type="text" size="small" @click="seeBtn(scope.$index, scope.row)">预览</el-button>
						</div>
					</el-col>
					<el-col :xs="8" :sm="8" :md="8" :lg="8">
						<div class="shopstyle"></div>
						<div style="margin-left: 60px">
							<el-button type="text" size="small" @click="seeBtn(scope.$index, scope.row)">使用</el-button>
							<el-button style="margin-left: 40px" type="text" size="small" @click="seeBtn(scope.$index, scope.row)">预览</el-button>
						</div>
					</el-col>
				</el-col>
				<el-col :xs="24" :sm="24" :md="24" :lg="24">
					<el-col :xs="24" :sm="24" :md="24" :lg="24">模块二</el-col>
					<el-col :xs="8" :sm="8" :md="8" :lg="8">
						<div class="shopstyle"></div>
						<div style="margin-left: 60px">
							<el-button type="text" size="small" @click="seeBtn(scope.$index, scope.row)">使用</el-button>
							<el-button style="margin-left: 40px" type="text" size="small" @click="seeBtn(scope.$index, scope.row)">预览</el-button>
						</div>
					</el-col>
					<el-col :xs="8" :sm="8" :md="8" :lg="8">
						<div class="shopstyle"></div>
						<div style="margin-left: 60px">
							<el-button type="text" size="small" @click="seeBtn(scope.$index, scope.row)">使用</el-button>
							<el-button style="margin-left: 40px" type="text" size="small" @click="seeBtn(scope.$index, scope.row)">预览</el-button>
						</div>
					</el-col>
					<el-col :xs="8" :sm="8" :md="8" :lg="8">
						<div class="shopstyle"></div>
						<div style="margin-left: 60px">
							<el-button type="text" size="small" @click="seeBtn(scope.$index, scope.row)">使用</el-button>
							<el-button style="margin-left: 40px" type="text" size="small" @click="seeBtn(scope.$index, scope.row)">预览</el-button>
						</div>
					</el-col>
				</el-col>
			</el-col>
		    <el-col :xs="9" :sm="9" :md="9" :lg="9">
		    	预览
		    </el-col>
		</el-row>
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
		          label: '全部'
		        }, {
		          value: '1',
		          label: '订单编号'
		        }, {
		          value: '2',
		          label: '快递单号'
		        }, {
		          value: '3',
		          label: '用户名'
		        }],
				filters: {
					name: '',
					status:'',
					type:''
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
				orderInformation:[{
					orderNumber :'145877458784524c',
					courierNumber :'145877458784524c',
					userName:'吸引力量',
					amountPaid :'300',
					orderTotal :'900',
					orderStatus :'待付款',
					paymentMethod :'微信支付',
					creationTime:'2017-09-08 17:09',
					deliveryTime:'2017-09-08 17:09',
					commodityName:'雨花说'
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
				this.$refs.editForm.validate((valid) => {
					if (valid) {
						this.$confirm('确认提交吗？', '提示', {}).then(() => {
							this.editLoading = true;
							//NProgress.start();
							let para = Object.assign({}, this.editForm);
							para.birth = (!para.birth || para.birth == '') ? '' : util.formatDate.format(new Date(para.birth), 'yyyy-MM-dd');
							editUser(para).then((res) => {
								this.editLoading = false;
								//NProgress.done();
								this.$message({
									message: '提交成功',
									type: 'success'
								});
								this.$refs['editForm'].resetFields();
								this.editFormVisible = false;
								this.getUsers();
							});
						});
					}
				});
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
.shopstyle{
	width: 200px;
	height: 200px;
	border: 1px solid #ddd;
	margin-left: 20px;
	margin-top: 20px;
	margin-bottom: 20px
}
</style>
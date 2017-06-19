<template>
	<section>
		<el-col :span="24" v-show="next">
			<el-col :span="24" class="Choice_div">
				<el-col :span="6" class="Choice">
					<el-tree :data="data" @node-click="handleNodeClick" :highlight-current="true"></el-tree>
				</el-col>
				<el-col :span="6" class="Choice">
				
				</el-col>
				<el-col :span="6" class="Choice">
				
				</el-col>
				<el-col :span="6" class="Choice">
				
				</el-col>
			</el-col>
			<el-col :span="24" class="footer_choice">
				您当前选择的商品是：{{commodity}}
			</el-col>
			<el-col :span="4" :offset="10">
				<el-button type="primary" style="width: 250px;margin-top: 20px" v-on:click="NextStep">下一步</el-button>
			</el-col>
		</el-col>
		<!--商品基本信息-->
		<el-col :span="24" v-show="details">
			<el-col :span="24" class="Commodity_information">
				<el-col :span="24" class="head_text">商品基本信息</el-col>
				<el-col :span="24" class="head_text">
					<el-col :span="2" class="leftg_text">商品类目：</el-col>当前分类{{commodity}}
				</el-col>
				<el-col :span="24" class="head_text">
					<el-col :span="2" class="leftg_text">商品品牌：</el-col>
				</el-col>
				<el-col :span="24" :offset="2" style="margin-top: 10px">宝贝属性 <span  style="color: #aaa"> 错误填写宝贝属性，可能会引起宝贝下架或搜索流量减少，影响您的正常销售，请确认准确填写！</span></el-col>
				<el-col :span="18" :offset="3" class="body_text">
					<el-col :span="14" :offset="4" style="margin-top: 10px">
						<el-form :model="orderDetails" label-width="120px" :rules="editFormRules" :inline="true" ref="editForm">
							<el-form-item label="货号">
								<el-input v-model="addForm.name" type="text" auto-complete="off"></el-input>
							</el-form-item>
							<el-form-item label="是否配包">
								<el-input v-model="addForm.name" type="text" auto-complete="off"></el-input>
							</el-form-item>
							<el-form-item label="品牌">
								<el-input v-model="addForm.name" type="text" auto-complete="off"></el-input>
							</el-form-item>
							<el-form-item label="质地">
								<el-input v-model="addForm.name" type="text" auto-complete="off"></el-input>
							</el-form-item>
							<el-form-item label="是否有扩展层">
								<el-input v-model="addForm.name" type="text" auto-complete="off"></el-input>
							</el-form-item>
							<el-form-item label="性别">
								<el-input v-model="addForm.name" type="text" auto-complete="off"></el-input>
							</el-form-item>
							<el-form-item label="适用对象">
								<el-input v-model="addForm.name" type="text" auto-complete="off"></el-input>
							</el-form-item>
							<el-form-item label="图案">
								<el-input v-model="addForm.name" type="text" auto-complete="off"></el-input>
							</el-form-item>
							<el-form-item label="重量">
								<el-input v-model="addForm.name" type="text" auto-complete="off"></el-input>
							</el-form-item>
							<el-form-item label="成色">
								<el-input v-model="addForm.name" type="text" auto-complete="off"></el-input>
							</el-form-item>
							<el-form-item label="锁的类型">
								<el-input v-model="addForm.name" type="text" auto-complete="off"></el-input>
							</el-form-item>
							<el-form-item label="闭合方式">
								<el-input v-model="addForm.name" type="text" auto-complete="off"></el-input>
							</el-form-item>
							<el-form-item label="有无拉杆">
								<el-input v-model="addForm.name" type="text" auto-complete="off"></el-input>
							</el-form-item>
							<el-form-item label="内部结构">
								<el-input v-model="addForm.name" type="text" auto-complete="off"></el-input>
							</el-form-item>
							<el-form-item label="是否带锁">
								<el-input v-model="addForm.name" type="text" auto-complete="off"></el-input>
							</el-form-item>
						<el-col :span='24'></el-col>
					</el-form>
					</el-col>
					
				</el-col>
				<el-col :span="24" class="shangp">
					<el-col :span="24" style=" border-bottom: 1px solid #ddd;">
						<el-col :span="2" style="border-right: 1px solid #ddd;height: 60px;text-align: right;line-height: 60px;">商品名称：</el-col>
						<el-col :span="12" style="margin-left: 10px">
							<el-col :span="12" style="margin-top: 10px"><el-input type="text""></el-input></el-col>
							<el-col :span="13"  style="margin-top: 5px;color: #aaa;">商品标题名称长度至少3个字符，最长50个汉字</el-col>
						</el-col>
					</el-col>
					<el-col :span="24" style=" border-bottom: 1px solid #ddd;">
						<el-col :span="2" style="border-right: 1px solid #ddd;height: 80px;text-align: right;line-height: 80px;">商品货号：</el-col>
						<el-col :span="12" style="margin-left: 10px">
							<el-col :span="12" style="margin-top: 10px"><el-input type="text""></el-input></el-col>
							<el-col :span="13"  style="margin-top: 5px;color: #aaa;">商品货号是指卖家个人管理商品的编号，买家不可见</el-col>
							<el-col :span="13"  style="margin-top: 5px;color: #aaa;">做多可输入20个字符，支持输入中文、字母、数字、_、/、和小数点</el-col>
						</el-col>
					</el-col>
					<el-col :span="24" style=" border-bottom: 1px solid #ddd;">
						<el-col :span="2" style="border-right: 1px solid #ddd;height: 280px;text-align: right;line-height: 80px;">商品图片：</el-col>
						<el-col :span="12" style="margin-left: 10px">
							<el-col :span="12" style="margin-top: 10px"><el-input type="text""></el-input></el-col>
							<el-col :span="19"  style="margin-top: 5px;color: #aaa;"><el-input type="textarea" :rows="10"></el-input></el-col>
						</el-col>
					</el-col>
					<el-col :span="24">
						<el-col :span="24" style="height: 40px;line-height: 30px;">商品交易信息</el-col>
						<el-col :span="24" style="height: 40px;line-height: 30px;border: 1px solid #ddd;">
							<el-col :span="2"  style="height: 30px;line-height: 30px;border-right: 1px solid #ddd;text-align: right">计量单位：</el-col>
							<el-col :span="2"  style=""><el-input type="text""></el-input></el-col>
						</el-col>
						<el-col :span="24" style="height: 100px;line-height: 30px;border: 1px solid #ddd;">
							<el-col :span="2"  style="height: 100px;line-height: 30px;border-right: 1px solid #ddd;text-align: right">尺码：</el-col>
							<el-col :span="20"  style="">
								<el-checkbox-group v-model="checkList">
								    <el-col :span="5" :offset="1">
								    	<el-checkbox label="L"></el-checkbox>
								    </el-col>
								    <el-col :span="5">
								    	<el-checkbox label="M"></el-checkbox>
								    </el-col>
								    <el-col :span="5">
								    	<el-checkbox label="S"></el-checkbox>
								    </el-col>
								    <el-col :span="5">
								    	<el-checkbox label="35"></el-checkbox>
								    </el-col>
								    <el-col :span="5"  :offset="1">
								    	<el-checkbox label="36"></el-checkbox>
								    </el-col>
								    <el-col :span="5">
								    	<el-checkbox label="37"></el-checkbox>
								    </el-col>
								    <el-col :span="5">
								    	<el-checkbox label="38"></el-checkbox>
								    </el-col>
								    <el-col :span="5">
								    	<el-checkbox label="39"></el-checkbox>
								    </el-col>
								</el-checkbox-group>
									<el-col :span="4">
								    	<el-input type="text""></el-input>
								    </el-col>
								    <el-col :span="5" :offset="1">
								    	<el-button type="primary" v-on:click="getUsers">添加规格值</el-button>
								    </el-col>
							</el-col>
						</el-col>
						<el-col :span="24" style="height: 150px;line-height: 30px;border: 1px solid #ddd;">
							<el-col :span="2"  style="height: 150px;line-height: 30px;border-right: 1px solid #ddd;text-align: right">规格报价：</el-col>
							<el-col :span="14"  style="">
								 <el-table :data="tableData" style="width: 100%">
							      <el-table-column prop="name" label="规格" width="180">
							      	<template scope="scope">
							      		<el-input v-model="scope.row.name" type="text"></el-input>
									</template>
							      </el-table-column>
							      <el-table-column prop="name" label="单价(元)" width="180">
							      	<template scope="scope">
							      		<el-input v-model="scope.row.name" type="text"></el-input>
									</template>
							      </el-table-column>
							      <el-table-column prop="address" label="建议零售价(元)" width="180">
							      	<template scope="scope">
							      		<el-input v-model="scope.row.name" type="text"></el-input>
									</template>
							      </el-table-column>
							      <el-table-column prop="date" label="单品货号" width="190">
								      <template scope="scope">
								      		<el-input v-model="scope.row.name" type="text"></el-input>
										</template>
							      </el-table-column>
							      <el-table-column prop="date" label="金豆抵扣" width="200">
							      	<template scope="scope">
							      		<el-input v-model="scope.row.name" type="text"></el-input>
									</template>
							      </el-table-column>
							    </el-table>
							</el-col>
						</el-col>
						<el-col :span="24" style="height: 100px;line-height: 30px;border: 1px solid #ddd;">
							<el-col :span="2"  style="height: 100px;line-height: 30px;border-right: 1px solid #ddd;text-align: center">定价模式 </el-col>
							<el-col :span="20">
								<el-col :span="20">
									 <el-radio class="radio" v-model="radio" label="1">模式一</el-radio>
 									 <el-radio class="radio" v-model="radio" label="2">模式二</el-radio>
								</el-col>
								<el-col :span="20">
									 模式一：分销金额 = （定价-成本（人民币））*吸引力平台分佣方式
								</el-col>
								<el-col :span="20">
									 模式二：分销金额 = （定价-成本（金豆））
								</el-col>
							</el-col>
						</el-col>
						<el-col :span="24" style=" border-bottom: 1px solid #ddd;">
							<el-col :span="2" style="border-right: 1px solid #ddd;height: 80px;text-align: left;line-height: 80px;">建议零售价(原价)：</el-col>
							<el-col :span="12" style="margin-left: 10px">
								<el-col :span="12" style="margin-top: 10px"><el-input type="text""></el-input></el-col>
								<el-col :span="13"  style="margin-top: 5px;color: #aaa;">商品价格必须是0.01~10000000的、之间的数字</el-col>
							</el-col>
						</el-col>
						<el-col :span="24" style="height: 40px;line-height: 30px;border-bottom: 1px solid #ddd;">商品详情描述</el-col>
						<el-col :span="24" style=" border-bottom: 1px solid #ddd;">
							<el-col :span="2" style="border-right: 1px solid #ddd;height: 400px;text-align: right;line-height: 80px;">商品描述：</el-col>
							<el-col :span="12" style="margin-left: 10px">
								<el-col :span="13"  style="margin-top: 5px;color: #aaa;">商品价格必须是0.01~10000000的、之间的数字</el-col>
							</el-col>
						</el-col>
						<el-col :span="24" style="height: 40px;line-height: 30px;border-bottom: 1px solid #ddd;">商品物流信息</el-col>
						<el-col :span="24">
							物流设置 为了提升消费者购物体验，要求客户全网商品设置运费模板。
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
				tableData: [{
		            date: '2016-05-02',
		            name: '王小虎',
		            address: '上海市普陀区金沙江路 1518 弄'
		          }, {
		            date: '2016-05-04',
		            name: '王小虎',
		            address: '上海市普陀区金沙江路 1517 弄'
		          }],
				radio: '1',
				commodity:'',
				next: true,
				details: false,
				value:'',
				value1:'',
				value2:'',
				data: [
				{
					id:'0',
					label:'家居日用'
				},{
					id:'1',
					label:'食品酒类'
				},{
					id:'2',
					label:'时尚穿戴'
				},{
					id:'3',
					label:'美妆个护'
				},{
					id:'4',
					label:'黑科技'
				},{
					id:'5',
					label:'营养保健'
				},{
					id:'6',
					label:'家居建材'
				},{
					id:'6',
					label:'数码通讯'
				},{
					id:'6',
					label:'家用电器'
				},{
					id:'6',
					label:'汽车'
				},{
					id:'6',
					label:'水果节'
				},{
					id:'6',
					label:'生鲜'
				},{
					id:'6',
					label:'百货'
				},{
					id:'6',
					label:'零食'
				}],
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
				addForm:{
					name:''
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
			handleNodeClick(data) {
	        	// console.log(data);
	        	this.commodity =  data.label
	        	console.log(this.commodity);
	     	},
	     	NextStep() {
	     		this.$confirm('类目一旦选定，编辑商品的时候类目无法更改，请确认清楚再进入编辑?', '提示', {
					type: 'warning'
				}).then(() => {
					this.next = false
					this.details = true
					// $.post(sysUrl+"/backstage/resources/update",
		   //           { param: JSON.stringify(params) },
		   //           function(data){
		   //           	const info = eval('(' + data + ')');
		   //              const response = JSON.parse(info);
		   //              console.log(response)
		   //           	if(response.res === 1000000){
					// 		_this.$message({
					// 			message: '修改成功',
					// 			type: 'success'
					// 		});
		   //           	}else{
					// 		_this.$message({
					// 			message: '修改失败',
					// 			type: 'error'
					// 		});
		   //           	}
		   //           	_this.getChildrenList(_this.pID)
					// 	_this.editFormVisible1 = false;
		   //            }
		   //       	)
				}).catch(() => {

				});
	     	},
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
	.Choice_div{
		width: 1600px;
		height: 500px;
		border: 1px solid #ddd;
		margin-top: 20px;
		margin-left: 20px;
	}
	.Choice{
		width: 350px;
		height: 400px;
		border: 1px solid #ddd;
		margin-left: 40px;
		margin-top: 40px;
		overflow: auto;
	}
	.footer_choice{
		width: 1600px;
		height: 40px;
		border: 1px solid #ddd;
		margin-top: 20px;
		margin-left: 20px;
		line-height: 40px;
		padding-left: 20px;
	}
	.Commodity_information{
		width: 1600px;
		height: 2400px;
		border: 1px solid #ddd;
		margin-top: 20px;
		margin-left: 20px;
	}
	.head_text{
		height: 40px;
		border-bottom: 1px solid #ddd;
		line-height: 40px;
	}
	.leftg_text{
		height:40px;
		border-right: 1px solid #ddd;
		text-align: right;
	}
	.body_text{
		border: 1px solid #ddd;
		height: 600px;
		margin-top: 20px;
	}
	.shangp{
		height:140px;
		border-top: 1px solid #ddd;
		margin-top: 10px;
	}
</style>
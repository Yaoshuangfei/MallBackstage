<template>
	<section>
		<el-col :span="24" v-show="template">
			<!--工具条-->
			<el-col :span="24" class="toolbar" style="padding-bottom: 0px;background: #fff">
				<el-form :inline="true">
					<el-form-item>
						<el-button type="primary" v-on:click="addTemplate">新增运费模板</el-button>
					</el-form-item>
				</el-form>
			</el-col>
			<el-col :span="24" style="margin-bottom: 20px">件数模板</el-col>
			<!--列表-->
			<el-col :span="24" style="border: 1px solid #ddd;height: 40px;line-height: 40px">
				<el-col :span="1" :offset="1" style="">
					1
				</el-col>
				<el-col :span="7" :offset="15" style="">
					最后编辑时间：2017-09-09 09:09:09 <span class="btncolor">设为默认的运费模板 | </span> <span class="btncolor">修改 | </span> <span class="btncolor">删除</span>
				</el-col>
			</el-col>
			<el-table :data="orderInformation" highlight-current-row v-loading="listLoading" style="width: 100%;min-width: 1080px;">
				<el-table-column prop="orderNumber" label="运送方式">
				</el-table-column>
				<el-table-column prop="courierNumber" label="运送到">
				</el-table-column>
				<el-table-column prop="userName" label="首件（个）">
				</el-table-column>
				<el-table-column prop="amountPaid" label="运费（元）">
				</el-table-column>
				<el-table-column prop="orderTotal" label="续件（个）">
				</el-table-column>
				<el-table-column prop="orderStatus" label="运费（元）">
				</el-table-column>
			</el-table>
		</el-col>
		

		<el-col :span="24" v-show="addtemplate">
			<el-col :span="24"><h3>新增运费模板</h3></el-col>
			<el-col :span="24">
				<el-col :span="2" class="left_temp">模板名称：</el-col>
				<el-col :span="3"><el-input v-model="addfrom.name" type="text"></el-input></el-col>
			</el-col>
			<el-col :span="24" class="top_margin">
				<el-col :span="2" class="left_temp">宝贝地址：</el-col>
				<el-col :span="8">
					<el-cascader
				    :options="options"
				    v-model="addfrom.areaId"
				    @change="handleChange">
				  </el-cascader>
				</el-col>
			</el-col>
			<el-col :span="24" class="top_margin">
				<el-col :span="2" class="left_temp">发货时间：</el-col>
				<el-col :span="6">
					<el-select v-model="addfrom.deliverTime" placeholder="请选择">
					    <el-option
					      v-for="item in times"
					      :key="item.value"
					      :label="item.label"
					      :value="item.value">
					    </el-option>
					  </el-select>
				</el-col>
			</el-col>
			<el-col :span="24" class="top_margin">
				<el-col :span="2" class="left_temp">是否包邮：</el-col>
				<el-col :span="6">
					<el-radio class="radio" v-model="addfrom.isFree" label="0">自定义运费</el-radio>
  					<el-radio class="radio" v-model="addfrom.isFree" label="1">卖家承担运费</el-radio>
				</el-col>
			</el-col>
			<el-col :span="24" class="top_margin">
				<el-col :span="2" class="left_temp">计件方式：</el-col>
				<el-col :span="6">
					<el-radio class="radio" v-model="addfrom.valuationType" label="0">按件数</el-radio>
  					<el-radio class="radio" v-model="addfrom.valuationType" label="1">安重量</el-radio>
  					<el-radio class="radio" v-model="addfrom.valuationType" label="2">按体积</el-radio>
				</el-col>
			</el-col>
			<el-col :span="24" class="top_margin">
				<el-col :span="2" class="left_temp">运送方式：</el-col>
				<el-col :span="6" style="line-height: 30px">除指定地区外，其余地区的运费采用“默认运费”</el-col>
			</el-col>
			<el-col :span="24">
				<el-col :span="2" :offset="2"> <el-checkbox v-model="checked">快递</el-checkbox></el-col>
			</el-col>
			<el-col :span="24" class="top_margin">
				<el-col :span="20" :offset="2" style="border:1px solid #ddd">
					<el-col :span="24" class="top_margin">
						<el-col :span="2" class="left_temp">默认运费：</el-col>
						<el-col :span="3"><el-input type="text"></el-input></el-col>
						<el-col :span="1" class="left_temp">kg内，</el-col>
						<el-col :span="3"><el-input type="text"></el-input></el-col>
						<el-col :span="1" style="line-height: 30px">元，</el-col>
						<el-col :span="1" class="left_temp">每增加</el-col>
						<el-col :span="3"><el-input type="text"></el-input></el-col>
						<el-col :span="1" style="line-height: 30px">kg，</el-col>
						<el-col :span="1" class="left_temp">增加运费</el-col>
						<el-col :span="3"><el-input type="text"></el-input></el-col>
						<el-col :span="1" style="line-height: 30px">元</el-col>
					</el-col>
					<el-col :span="20" class="top_margin" style="margin-left: 10px;">
						<el-table :data="fareCarries" >
					      <el-table-column prop="data" label="运送到" width="280">
					      	<template scope="scope">
					      		{{scope.row.data}}
					      		<el-button type="text" size="small" @click="handleEdit(scope.$index, scope.row)">编辑</el-button>
							</template>
					      </el-table-column>
					      <el-table-column prop="firstHeavy" label="首重(kg)" width="180">
					      	<template scope="scope">
					      		<el-input v-model="scope.row.firstHeavy" type="text"></el-input>
							</template>
					      </el-table-column>
					      <el-table-column prop="firstPrice" label="首费(元)" width="180">
					      	<template scope="scope">
					      		<el-input v-model="scope.row.firstPrice" type="text"></el-input>
							</template>
					      </el-table-column>
					      <el-table-column prop="addHeavy" label="续重(kg)" width="190">
						      <template scope="scope">
						      		<el-input v-model="scope.row.addHeavy" type="text"></el-input>
								</template>
					      </el-table-column>
					      <el-table-column prop="addPrice" label="续费(元)" width="200">
					      	<template scope="scope">
					      		<el-input v-model="scope.row.addPrice" type="text"></el-input>
							</template>
					      </el-table-column>
					      <el-table-column label="操作">
							<template scope="scope">
								<el-button type="text" size="small" @click="handleEdit(scope.$index, scope.row)">删除</el-button>
							</template>
						</el-table-column>
					    </el-table>
					</el-col>
					<el-col :span="20" class="top_margin" style="margin-left: 10px;">
						<el-button type="text" size="small" @click="handleEdit(scope.$index, scope.row)">为指定地区设置运费</el-button>
						<el-button type="text" size="small" @click="handleEdit(scope.$index, scope.row)">批量操作</el-button>
					</el-col>
				</el-col>
			</el-col>
			<el-col :span="24" class="top_margin">
				<el-col :span="2" :offset="2"> <el-checkbox v-model="ems">EMS</el-checkbox></el-col>
			</el-col>
			<el-col :span="24" class="top_margin">
				<el-col :span="2" :offset="2"> <el-checkbox v-model="pingyou">平邮</el-checkbox></el-col>
			</el-col>
			<el-col :span="24" class="top_margin">
				<el-col :span="2" :offset="1"> <el-checkbox v-model="addfrom.specifyMailStatus">按指定条件包邮可选</el-checkbox></el-col>
			</el-col>
			<el-col :span="24" class="top_margin">
				<el-col :span="16" :offset="1">
					<el-table :data="tableData" >
					      <el-table-column prop="name" label="选择地区" width="280">
					      	<template scope="scope">
					      		{{scope.row.name}}
					      		<el-button type="text" size="small" @click="handleEdit(scope.$index, scope.row)">编辑</el-button>
							</template>
					      </el-table-column>
					      <el-table-column prop="name" label="选择运送方式" width="180">
					      	<template scope="scope">
					      		<el-input v-model="scope.row.name" type="text"></el-input>
							</template>
					      </el-table-column>
					      <el-table-column prop="address" label="选择快递" width="180">
					      	<template scope="scope">
					      		<el-input v-model="scope.row.name" type="text"></el-input>
							</template>
					      </el-table-column>
					      <el-table-column prop="date" label="设置包邮条件" width="190">
						      <template scope="scope">
						      		<el-input v-model="scope.row.name" type="text"></el-input>
								</template>
					      </el-table-column>
					      <el-table-column label="操作">
							<template scope="scope">
								<el-button type="text" size="small" @click="handleEdit(scope.$index, scope.row)">删除</el-button>
							</template>
						</el-table-column>
					    </el-table>
				</el-col>
			</el-col>
			<el-col :span="24" class="top_margin"><el-button type="primary" v-on:click="preservation">保存</el-button></el-col>
		</el-col>

		<!--工具条-->
		<!-- <el-col :span="24" class="toolbar" style="background:#fff;"> -->
			<!-- <el-button type="danger" @click="batchRemove" :disabled="this.sels.length===0">批量删除</el-button> -->
			<!-- <el-pagination layout="prev, pager, next" @current-change="handleCurrentChange" :page-size="10" :total="total" style="float:right;">
			</el-pagination>
		</el-col> -->
	</section>
</template>

<script>
	import { state } from '../../vuex/state'
	import util from '../../common/js/util'
	import {baseUrl , editUser, addUser,cityData3} from '../../api/api';

	export default {
		data() {
			return {
				addfrom:{
					name:'',
					areaId:[],
					deliverTime:'',
					isFree:'0',
					valuationType:'0',
					specifyMailStatus:false,
					storeId:'5'
				},
				fareCarries:[{
					data:'',
					firstHeavy:'',
					firstPrice:'',
					addHeavy:'',
					addPrice:'',
					mode:'0',
					isDefault:'0'
				}],
				isareaId:'',
				fareIpps:[{
					ippType:'',
					ippData:'',
					ippNum:'',
					ippPrice:'',
					transportType:''
				}],
				template:true,
				addtemplate:false,
				radio: '1',
				piece:'1',
				checked: true,
				pingyou:false,
				ems:false,
				tableData:[{
					name:'1'
				}],
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
				options: cityData3,
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
				}],
				times:[{
		          value: '4',
		          label: '4小时内'
		        }, {
		          value: '8',
		          label: '8小时内'
		        }, {
		          value: '12',
		          label: '12小时内'
		        }, {
		          value: '16',
		          label: '16小时内'
		        }, {
		          value: '20',
		          label: '20小时内'
		        }, {
		          value: '24',
		          label: '1天内'
		        }, {
		          value: '48',
		          label: '2天内'
		        }, {
		          value: '144',
		          label: '12天内'
		        }, {
		          value: '225',
		          label: '15天内'
		        }, {
		          value: '408',
		          label: '17天内'
		        }, {
		          value: '480',
		          label: '20天内'
		        }, {
		          value: '600',
		          label: '25天内'
		        }]
			}
		},
		methods: {
			//新增模板
			addTemplate() {
				this.template = false
				this.addtemplate = true
			},
			 handleChange(value) {
		        console.log(value);
		        this.isareaId = value[value.length-1]
		        console.log(this.addfrom.areaId)
		      },
			//性别显示转换
			formatSex: function (row, column) {
				return row.sex == 1 ? '男' : row.sex == 0 ? '女' : '未知';
			},
			preservation() {
				const _this = this
				// const _length =  _this.addfrom.areaId.length
				// const isareaId = _this.addfrom.areaId[_length]
				const params = {
					name:_this.addfrom.name,
					areaId:this.isareaId,
					deliverTime:_this.addfrom.deliverTime,
					isFree:_this.addfrom.isFree,
					valuationType:_this.addfrom.valuationType,
					specifyMailStatus:specifyMailStatu,
					storeId:_this.addfrom.storeId,
					fareCarries:[]
				}
				let specifyMailStatu = 0
				let check
				if(_this.addfrom.specifyMailStatus === false){
					specifyMailStatu
				}else{
					specifyMailStatu = 1
				}
				if(this.checked === false){
					check
				}else{
					check = 0
					const fareCarrie = {
						data:_this.fareCarries[0].data,
						firstHeavy:_this.fareCarries[0].firstHeavy,
						firstPrice:_this.fareCarries[0].firstPrice,
						addHeavy:_this.fareCarries[0].addHeavy,
						addPrice:_this.fareCarries[0].addPrice,
						mode:check,
						isDefault:0
					}
					params.fareCarries.push(fareCarrie)
				}
				
				console.log(params)
				this.addtemplate = false
				this.template = true
				// $.post(baseUrl+"/admin/banner/getBannerByPage",
	   //           { param: JSON.stringify(params) },
	   //           function(data){
	   //           	const info = eval('(' + data + ')');
	   //              const response = JSON.parse(info);
	   //              const list = response.obj.results
	   //              console.log(response)
	   //              // _this.page = response.obj.total
	   //              _this.total = response.obj.totalRecord
	   //              for(var i = 0;i<list.length;i++){
	   //              	_this.table.push(list[i])
	   //              }
	   //            }
	   //       	)
			},
			handleCurrentChange(val) {
				this.page = val;
				this.getUsers();
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
	.btncolor{
		color: #20a0ff;
		cursor: pointer;
	}
	.left_temp{
		text-align: right;
		line-height: 30px;
	}
	.top_margin{
		margin-top: 20px;
	}
</style>
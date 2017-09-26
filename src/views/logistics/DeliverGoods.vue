<template>
	<section>
		<!--工具条-->

		<el-col :span="24" class="toolbar" style="padding-bottom: 0px;background: #fff">
			<el-form :inline="true" :model="filters">
				<!-- <el-form-item>
					<el-input v-model="filters.name" placeholder="支付银行"></el-input>
				</el-form-item> -->
				<!-- <el-form-item label="状态">
					<el-select v-model="filters.status" clearable>
				      <el-option v-for="item in selectSubjectStatus" :label="item.label" :value="item.value">
				      </el-option>
				    </el-select>
				</el-form-item> -->
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
					<el-button type="primary" v-on:click="getlist">查询</el-button>
					<el-button type="primary" v-on:click="getUsers">导出订单</el-button>
				</el-form-item>
			</el-form>
		</el-col>
		<el-col :span="24" style="background: #cab78c;color: #fff;font-size: 16px;height:48px;line-height: 48px;text-align: center;">
			<el-col style="width:340px;">商品详情</el-col>
			<el-col style="width:98px;">单价</el-col>
			<el-col style="width:90px;">数量</el-col>
			<el-col style="width:137px;">买家</el-col>
			<el-col style="width:180px;">订单总价</el-col>
			<el-col style="width:84px;">状态</el-col>
		</el-col>
		<el-col :span="24" class="table_div" v-for="item in selectSubjectStatus">
			<el-col :span="24"  class="table_div_head">
				<el-col :span="6">订单编号：{{item.id}}</el-col>
				<el-col :span="7">下单时间：{{item.createTime}}</el-col>
				<el-col :span="3" :offset="1" v-if="item.coreUser === null">/</el-col>
				<el-col :span="3" :offset="1" v-if="item.coreUser !== null">{{item.coreUser.nickName}}</el-col>
				<el-col :span="1"  style="margin-left:50px">{{item.totalMoney}}</el-col>
				<el-col :span="2"  style="margin-left:	10px">
					<router-link :to="{ name: '订单详情', params: { id: item.id ,index: 1 }}">
						<el-button style="margin-top:-5px;color: #9f3333;"  type="text">查看订单</el-button>
					</router-link>
					
				</el-col>
				<el-col :span="1" style="margin-top: -5px;">
					<el-button type="text" v-on:click="fahuoBtn(item.orderGoods)" style="color: #9f3333;">发货</el-button>
					<!-- <el-button v-if="item.orderGoods.orderStatus === 2" type="text" v-on:click="fahuoBtn(item.orderGoods)" style="color: #9f3333;" disabled>发货</el-button> -->
				</el-col>
			</el-col>
			<el-col :span="24" v-for="items in item.orderGoods">
				<el-col :span="3" >
					<img style="width: 100px;margin-left:40px;margin-top: 20px " :src="items.picture">
				</el-col>
				<el-col  style="width:180px;margin-top: 40px;" :offset="1">
				{{items.productName}}
				</el-col>
				<el-col style="text-align: center;width:98px;margin-top: 40px;">{{items.productPrice}}</el-col>
				<el-col style="text-align: center;width:90px;margin-top: 40px;" >{{items.quantity}}</el-col>
				<el-col style="text-align: right;width:137px;margin-left: 240px;margin-top: 40px;">
					<!-- <el-button type="text" v-on:click="fahuoBtn(items)" style="color: #9f3333;">发货</el-button> -->
					<el-col v-if="items.refundStatus === 1">
						<el-col v-if="items.orderStatus === 2">待发货</el-col>
						<el-col v-else>已发货</el-col>
					</el-col>
					<el-col v-else>退款中</el-col>
				</el-col>
			</el-col>
			<el-col :span="24" style="border-top: 1px solid #e6eef9;height: 30px;line-height: 50px;padding-left: 20px;">地址：{{item.provinceName}}{{item.cityName}}{{item.countyName}}{{item.address}}</el-col>
		</el-col>
		<!--工具条-->
		<el-col :span="18" class="toolbar" style="background:#fff;">
			<!-- <el-button type="danger" @click="batchRemove" :disabled="this.sels.length===0">批量删除</el-button> -->
			<el-pagination layout="prev, pager, next" @current-change="handleCurrentChange" :page-size="10" :total="total" style="float:right;">
			</el-pagination>
		</el-col>

		<!--编辑界面-->
		<el-dialog title="发货" v-model="editFormVisible" :close-on-click-modal="false" >
			<el-col :span="24">
				<el-col :span="24" style="margin-bottom: 20px;">
					<el-col :span="24" v-show="goodsList.length > 0">
						<el-col :span="2" style="margin-right: 20px;">待发货</el-col>
						<el-checkbox-group v-model="checkedCities" @change="handleCheckedCitiesChange">
							<el-checkbox v-for="city in goodsList" :label="city.id" :key="city.id">{{city.productName}}</el-checkbox>
						</el-checkbox-group>
					</el-col>
					<el-col :span="24" v-show="goodsList1.length > 0">
						<el-col :span="2" style="margin-right: 20px;">已发货</el-col>
						<el-checkbox-group v-model="checkedCities1" @change="handleCheckedCitiesChange">
							<el-checkbox v-for="city in goodsList1" :label="city.id" :key="city.id" disabled>{{city.productName}}</el-checkbox>
						</el-checkbox-group>
					</el-col>
					<el-col :span="24" v-show="goodsList2.length > 0">
						<el-col :span="2" style="margin-right: 20px;">退款中</el-col>
						<el-checkbox-group v-model="checkedCities2" @change="handleCheckedCitiesChange">
							<el-checkbox v-for="city in goodsList2" :label="city.id" :key="city.id" disabled>{{city.productName}}</el-checkbox>
						</el-checkbox-group>
					</el-col>
				</el-col>
				<!-- <el-col :span="24" style="margin-bottom: 20px">
					<ci-col :span="4">
						发货方式：
					</ci-col>
					<ci-col :span="4">
						<el-select v-model="huovalue" clearable>
					      <el-option v-for="item in option" :label="item.label" :value="item.value">
					      </el-option>
					    </el-select>
					</ci-col>
				</el-col> -->
				<el-col :span="24" style="margin-bottom: 20px">
					<el-select v-model="keyvalue" clearable>
				      <el-option v-for="item in optionKey" :label="item.label" :value="item.value">
				      </el-option>
				    </el-select>
				</el-col>
				<el-col :span="24" style="margin-bottom: 20px">
					<el-input v-model='expno' @change="keyup"></el-input>
				</el-col>
			</el-col>
			<div slot="footer" class="dialog-footer" style="text-align: center;">
				<el-button type="primary" @click.native="upSubmit" :loading="editLoading">确认</el-button>
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
				huovalue:1,
				keyvalue: '',
				expno:'',
				ddId:'',
				dpId:'',
				checked: true,
				value:'',
				value1:'',
				value2:'',
				selectSubjectStatus: [],
				options: [{
		          value: '0',
		          label: '全部'
		        }, {
		          value: '1',
		          label: '订单编号'
		        }, {
		          value: '2',
		          label: '快递单号'
		        }],
		        option:[
		        	{value:1,label:'在线下单'},
		        	{value:1,label:'送货上门'}
		        ],
		        optionKey:[],
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
				orderInformation:[],
				goodsList:[],
				cities:[],
				checkedCities:[],
				goodsList1:[],
				checkedCities1:[],
				goodsList2:[],
				checkedCities2:[]
			}
		},
		methods: {
			handleCheckedCitiesChange(){

			},
			fahuoBtn(row){
				console.log(row)
				this.goodsList = []
				this.checkedCities = []
				this.goodsList1 = []
				this.checkedCities1 = []
				this.goodsList2 = []
				this.checkedCities2 = []
				for (var i = 0; i < row.length; i++) {
					if(row[i].refundStatus === 2){
						this.checkedCities2.push(row[i].id)
						this.goodsList2.push(row[i])
					}else{
						if(row[i].orderStatus === 2){
							this.checkedCities.push(row[i].id)
							this.goodsList.push(row[i])
						}else{
							this.checkedCities1.push(row[i].id)
							this.goodsList1.push(row[i])
						}
					}
				}
				this.optionKey = []
				this.keyvalue =''
				this.expno = ''
				this.editFormVisible = true
				this.ddId = row.id
				this.dpId = row.storeId
			},
			keyup(){
				const _this = this
				const params = {
					expno:this.expno
				}
				$.ajax({
                    type:'POST',
                    dataType:'json',
                    url:baseUrl+"/api/orderMall/orderTracesByJson",
                    data:JSON.stringify(params),
                    contentType:'application/json;charset=utf-8',
                    error: function (XMLHttpRequest, textStatus, errorThrown) {},
                    success:function(data){
                    	_this.optionKey = []
                    	if(data.data !== null && data.data !== []){
                    		for(var i = 0;i<data.data.length;i++){
                    			const obj = {}
                    			obj.value = data.data[i].ShipperCode
                    			obj.label = data.data[i].ShipperName
                    			_this.optionKey.push(obj)
                    			_this.keyvalue = obj.value
                    		}
                    	}
                    }
                });
			},
			upSubmit() {
				let express = ''
				for(var i = 0;i<this.optionKey.length;i++){
					if(this.keyvalue === this.optionKey[i].value){
						express = this.optionKey[i].label
					}
				}
				console.log(this.huovalue)
				console.log(this.checkedCities)
				const _this = this
				const params = []
				for (var i = 0; i < this.checkedCities.length; i++) {
					const obj = {
						id:this.checkedCities[i],
						deliveryType:1,
						expno:this.expno,
						expressName:express,
						expressCode:this.keyvalue
					}
					params.push(obj)
				}
				console.log(JSON.stringify(params))
				$.ajax({
                    type:'POST',
                    dataType:'json',
                    url:baseUrl+"/api/orderMall/deliveryAll",
                    data:JSON.stringify(params),
                    contentType:'application/json;charset=utf-8',
                    success:function(data){
                    	_this.editFormVisible = false
                    	_this.getlist()
                    }
                });
			},
			getlist(){
				const _this = this
				this.selectSubjectStatus = []
				const params = {
					pageNum:this.page,
					size:10,
//					storeId:state.storeId,
                    storeId:localStorage.getItem("storeId"),
					orderStatus:2,
					orderId:'',
					expno:''
				}
				if(this.filters.type !== ''){
					if(this.filters.type === '1'){
						params.orderId = this.filters.name
					}else if(this.filters.type === '2'){
						params.expno = this.filters.name
					}
				}
				$.ajax({
                    type:'POST',
                    dataType:'json',
                    url:baseUrl+"/api/orderMall/selectListAll",
                    data:JSON.stringify(params),
                    contentType:'application/json;charset=utf-8',
                    error: function (XMLHttpRequest, textStatus, errorThrown) {},
                    success:function(data){
                    	const info = data.data
                    	_this.total = info.total
                    	_this.selectSubjectStatus = info.list
                    	for(var i = 0;i<_this.selectSubjectStatus.length;i++){
                    		_this.selectSubjectStatus[i].createTime = new Date(_this.selectSubjectStatus[i].createTime).toLocaleString()
                    	}
                    }
                });
			},

			delivery(value,expno,expressName,expressCode){
				const _this = this
				const params = {
					id:value,
					storeId:state.storeId,
					expno:expno,
					expressName:expressName,
					expressCode:expressCode
				}
				$.ajax({
                    type:'POST',
                    dataType:'json',
                    url:baseUrl+"/api/orderMall/delivery",
                    data:JSON.stringify(params),
                    contentType:'application/json;charset=utf-8',
                    error: function (XMLHttpRequest, textStatus, errorThrown) {},
                    success:function(data){
                    }
                });
			},
			handleCurrentChange(val) {
				this.page = val;
				this.getlist();
			},
			//获取用户列表
			getUsers() {
				let para = {
					page: this.page,
					name: this.filters.name
				};
				this.listLoading = true;
				getUserListPage(para).then((res) => {
					this.total = res.data.total;
					this.users = res.data.users;
					this.listLoading = false;
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
			this.getlist();
		}
	}

</script>

<style>

</style>
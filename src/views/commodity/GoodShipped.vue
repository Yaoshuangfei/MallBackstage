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
				<span  style="display: none;">{{id}}</span>
			</el-col>
			<el-col :span="4" :offset="10">
				<el-button type="primary" style="width: 250px;margin-top: 20px" :disabled="this.sels.length===0"  v-on:click="NextStep">下一步</el-button>
			</el-col>
		</el-col>
		<!--商品基本信息-->
		<el-col :span="24" v-show="details">
			<el-col :span="24" class="Commodity_information">
				<el-col :span="24" class="head_text">商品基本信息</el-col>
				<el-col :span="24" class="head_text">
					<el-col :span="2" class="leftg_text">商品类目：</el-col>当前分类:     {{commodity}}
				</el-col>
				<el-col :span="24" class="head_text">
					<el-col :span="2" class="leftg_text">商品品牌：</el-col></el-col>
				<el-col :span="24" :offset="2" style="margin-top: 10px">宝贝属性 <span  style="color: #aaa"> 错误填写宝贝属性，可能会引起宝贝下架或搜索流量减少，影响您的正常销售，请确认准确填写！</span></el-col>
				<el-col :span="18" :offset="3" class="body_text">
					<el-col :span="19" :offset="2" style="margin-top: 10px">
						<el-form :model="orderDetails" label-width="80px" :rules="editFormRules" :inline="true" ref="editForm">
							<el-form-item :label="item.name" v-for="item in paramsData" style="margin-right:  200px;">
								<el-input type="text"></el-input>
							</el-form-item>
						<el-col :span='24'></el-col>
					</el-form>
					</el-col>
					
				</el-col>
				<el-col :span="24" class="shangp">
					<el-col :span="24" style=" border-bottom: 1px solid #ddd;">
						<el-col :span="2" style="border-right: 1px solid #ddd;height: 70px;text-align: right;line-height: 60px;">商品名称：</el-col>
						<el-col :span="12" style="margin-left: 10px">
							<el-col :span="12" style="margin-top: 10px"><el-input type="text"></el-input></el-col>
							<el-col :span="13"  style="margin-top: 5px;color: #aaa;">商品标题名称长度至少3个字符，最长50个汉字</el-col>
						</el-col>
					</el-col>
					<el-col :span="24" style=" border-bottom: 1px solid #ddd;">
						<el-col :span="2" style="border-right: 1px solid #ddd;height: 95px;text-align: right;line-height: 80px;">商品货号：</el-col>
						<el-col :span="12" style="margin-left: 10px">
							<el-col :span="12" style="margin-top: 10px"><el-input type="text"></el-input></el-col>
							<el-col :span="13"  style="margin-top: 5px;color: #aaa;">商品货号是指卖家个人管理商品的编号，买家不可见</el-col>
							<el-col :span="13"  style="margin-top: 5px;color: #aaa;">做多可输入20个字符，支持输入中文、字母、数字、_、/、和小数点</el-col>
						</el-col>
					</el-col>
					<el-col :span="24" style=" border-bottom: 1px solid #ddd;">
						<el-col :span="2" style="text-align: right;line-height: 80px;">商品图片：</el-col>
						<el-col :span="12" style="margin-left: -1px;padding-left:10px;border-left: 1px solid #ddd">
							<el-col :span="12" style="margin-top: 10px"><el-input type="text"></el-input></el-col>
							<el-col :span="19"  style="margin-top: 5px;color: #aaa;"><el-input type="textarea" :rows="10"></el-input></el-col>
						</el-col>
					</el-col>
					<el-col :span="24">
						<el-col :span="24" style="height: 40px;line-height: 30px;">商品交易信息</el-col>
						<el-col :span="24" style="height: 40px;line-height: 30px;border: 1px solid #ddd;">
							<el-col :span="2"  style="height: 30px;line-height: 30px;border-right: 1px solid #ddd;text-align: right">计量单位：</el-col>
							<el-col :span="2"  style=""><el-input type="text"></el-input></el-col>
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
								<el-col :span="12" style="margin-top: 10px"><el-input type="text"></el-input></el-col>
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
			<el-button type="primary" @click="release" style="margin: 20px;">发布</el-button>
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
                sels:[],
                paramsData:[],
                itemData:[],
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
                checkList:false,
				commodity:'',
				id:'',
				next: true,
				details: false,
				value:'',
				value1:'',
				value2:'',
				data: [],
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
                this.sels=[];
	        	this.commodity =  data.label;
	        	this.id =  data.id;
	        	this.sels.push(data);
	        	// this.sels.push(this.id);
	        	console.log(this.sels)
	     	},
	     	NextStep() {
	     		this.$confirm('类目一旦选定，编辑商品的时候类目无法更改，请确认清楚再进入编辑?', '提示', {
					type: 'warning'
				}).then(() => {

                    const _this = this;
                    const url   = baseUrl+"/api/goodsClass/selectOne";
                    const params = {
                        id:this.sels[0].id
                        // id:this.id
                    };
                    const data =JSON.stringify(params);
                    $.ajax({
                        type:'POST',
                        dataType:'json',
                        url:url,
                        data:data,
                        contentType:'application/json;charset=utf-8',
                        error: function (XMLHttpRequest, textStatus, errorThrown) {},
                        success:function(data){
                        	const info = data.data
                        	_this.paramsData = eval('(' + info.paramData + ')')
                        	_this.itemData = eval('(' + info.itemData + ')')
                        	console.log(_this.paramsData)
                        	console.log(_this.itemData)
                            // if(!data.success){
                            //     alert(data.msg)
                            // }else{
                            //     var _length =data.data.paramData;
                            //     var _name =eval('(' + _length + ')');
                            //     console.log(_name);
                            // }
        					_this.next = false;
							_this.details = true;
                        }
                    });
				}).catch(() => {

				});
	     	},
			//获取商品分类
            selectListName(){
                const _this = this;
                const url   = baseUrl+"/api/goodsClass/selectListName";
                const params = {
                    pid:''
                };
                const data =JSON.stringify(params);
                $.ajax({
                    type:'POST',
                    dataType:'json',
                    url:url,
                    data:data,
                    contentType:'application/json;charset=utf-8',
                    error: function (XMLHttpRequest, textStatus, errorThrown) {},
                    success:function(data){
                    	console.log(data)
                        if(!data.success){
                            alert(data.msg)
                        }else{
                            const _length = data.data;
                            for (const i in _length){
                                const obj = {value:i,label:_length[i].name,id:_length[i].id};
                                _this.data.push(obj);
                            }
                        }
                    }
                });
			},
			// 发布商品
			release() {
				const _this = this
                const params = {
                    name:'iPhone8',
                    goodsDesc:'iPhone8支持无线充电，内置900G内存',
                    veiw:'',
                    price:'8588',
                    isVirtual:'0',
                    carouselPicture:'icon.png,icon1.png',
                    saleState:'1',
                    goodsData:"[{'key':'品牌','value':'苹果'},{'key':'型号','value':'iPhone7，iPhone7plus'}]",
                    catId:'100',
                    storeId:'10',
                    ftId:'0',
                    pricingModel:'1',
                    unit:'件',
                    weight:'0.5',
                    goodsNo:'88888',
                    goodsSpecs:[{
                    	specPrice:'8888',
	                    costPrice:'7888',
	                    storage:'1000',
	                    specPicture:'icon3.png',
	                    deductibleImazamox:'10',
	                    specData:"[{'key':'颜色','value':'黑色'},{'key':'内存','value':'32g'}]",
	                    specNo:'8888-1'
                    }]
                };
                $.ajax({
                    type:'POST',
                    dataType:'json',
                    url:baseUrl+"/api/goods/add",
                    data:JSON.stringify(params),
                    contentType:'application/json;charset=utf-8',
                    error: function (XMLHttpRequest, textStatus, errorThrown) {},
                    success:function(data){
                    	console.log(data)
                    }
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
			 this.selectListName();
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
		min-height: 200px;
		margin-top: 20px;
	}
	.shangp{
		height:140px;
		border-top: 1px solid #ddd;
		margin-top: 10px;
	}
</style>
<template>
	<section>
		<el-button type="primary" v-on:click="addIDCard" style="margin-top: 20px" :disabled="this.identity.length===5">新增店铺身份</el-button>
		<el-row :gutter="10" style="margin-top: 40px">
		  <el-col :xs="2" :sm="2" :md="2" :lg="2">店铺身份</el-col>
		  <el-col :xs="15" :sm="15" :md="15" :lg="15">
		  		<el-col :xs="24" :sm="24" :md="24" :lg="24" v-for="item in identity" style="margin-bottom: 40px">
		  			<el-col :xs="4" :sm="4" :md="4" :lg="4">
		  				<img :src="item.icon">
		 			</el-col>
		 			<el-col :xs="4" :sm="4" :md="4" :lg="4">
		  				{{item.name}}
		 			</el-col>
		 		</el-col>
		  </el-col>
		</el-row>
		<!--列表-->
		<el-col :xs="14" :sm="14" :md="14" :lg="14" style="margin-top: 40px;margin-bottom: 20px">购买身份价格</el-col>
		<el-table :data="identity" highlight-current-row v-loading="listLoading" style="width: 50%;min-width: 580px;">
			<el-table-column type="index">
			</el-table-column>
			<el-table-column prop="name" label="身份">
			</el-table-column>
			<el-table-column prop="price" label="价格">
			</el-table-column>
			<el-table-column label="操作">
				<template scope="scope">
					<el-button type="text" size="small" @click="handleEdit(scope.$index, scope.row)">修改</el-button>
				</template>
			</el-table-column>
		</el-table>
		<el-col :xs="14" :sm="14" :md="14" :lg="14" style="margin-top: 40px;margin-bottom: 20px">商品照片</el-col>
		<el-col :xs="24" :md="24">
				<!-- 上传图片 -->
				<form style="position:relative;">
				    <input type="file" style="position:absolute;opacity:0;width:70px;height:30px;margin-right:10px"  @change="upload" id="fileInput">
				    <el-button type="button" class="el-button el-button--primary el-button--small" :disabled="this.imgArry.length===3">
				    	<span >点击上传</span>
				    </el-button>
				    <el-button type="button" class="el-button el-button--primary el-button--small" :disabled="this.imgArry.length===3" id="btnClear" @click="clear">清空上传</el-button>
				    <!-- <span style="display: block;font-size: 12px">{{ imageChange }}</span> -->
				</form>
		</el-col>
		<el-col :xs="24" :md="24"  style="margin-top: 20px">
			<el-col :xs="4" :md="4" v-for="item in imgArry">    <!-- arry[0].baseUri+arry[0].uri -->
				<img :src="item.baseUri+item.uri" style="width:200px;height: 200px;">
			</el-col>
		</el-col>
		<el-col :xs="14" :sm="14" :md="14" :lg="14" style="margin-top: 40px;margin-bottom: 20px">身份介绍</el-col>
		<el-input type="textarea" v-model="desc"></el-input>
		<el-col :xs="14" :sm="14" :md="14" :lg="14" style="margin-top: 40px;margin-bottom: 20px">
		<el-button type="primary" v-on:click="shopRoleAdd">发布</el-button>
		</el-col>
		<!--新增界面-->
		<el-dialog title="新增店铺身份" v-model="addFormVisible" :close-on-click-modal="false" >
			<el-form :model="orderDetails" label-width="160px" :rules="editFormRules" ref="editForm">
				<el-form-item label="角色名称：">
					<el-input v-model="orderDetails.name" type="text" auto-complete="off"></el-input>
				</el-form-item>
				<el-form-item label="价格：">
					<el-input v-model="orderDetails.price" type="text" auto-complete="off"></el-input>
				</el-form-item>
				<el-form-item label="角色图标：">
					<el-input v-model="orderDetails.icon" type="text" auto-complete="off"></el-input>
				</el-form-item>
				<el-col :span='24'></el-col>
			</el-form>
			<div slot="footer" class="dialog-footer" style="text-align: center;">
				<el-button type="primary" @click.native="addSubmit" :loading="editLoading">确定</el-button>
				<el-button type="primary" @click.native="addFormVisible = false">关闭</el-button>
			</div>
		</el-dialog>
		<!--编辑界面-->
		<el-dialog title="编辑购买身份价格" v-model="editFormVisible" :close-on-click-modal="false" >
			<el-form :model="editForm" label-width="160px" :rules="editFormRules" ref="editForm">
				<el-form-item label="角色名称：">
					{{editForm.name}}
				</el-form-item>
				<el-form-item label="价格：">
					<el-input v-model="editForm.price" type="text" auto-complete="off"></el-input>
				</el-form-item>
				<el-col :span='24'></el-col>
			</el-form>
			<div slot="footer" class="dialog-footer" style="text-align: center;">
				<el-button type="primary" @click.native="upedit" :loading="editLoading">确定</el-button>
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
				desc:'',
				radio: '0',
				checked: true,
				identity:[],
				imgArry:[],
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
				editForm: {},
				addFormVisible: false,//新增界面是否显示
				addLoading: false,
				//新增界面数据
				orderDetails: {
				},
				formData: new FormData(),
				fileImg: ''
			}
		},
		computed: {
		    // 实时更新上传图片的名字，仅读取，值只须为函数
		    imageChange: function () {
		      return this.fileImg
		    }
	    },
		methods: {
			clear(){
				let btn = document.getElementById("btnClear");
     			let files = document.getElementById("fileInput");
     			this.fileImg = '';
				// for IE, Opera, Safari, Chrome
		        if (files !== null && files.value) {
		        //     files.outerHTML = files.outerHTML;
		        // } else { // FF(包括3.5)
	            	files.value = "";
	            	this.formData = new FormData()
		        }
			},
			// 添加或编辑上传图片
			upload (event) {
				this.formData = new FormData()
                let file = event.target.files[0]
                console.log(this.formData)
                console.log(file)
                const self = this
                // const flag = this.flag
                if (file) {
                	console.log('存在file', file)
                	this.fileImg = file.name
                    // console.log(this.formData)
                    this.formData.append('file', file)
                    console.log(this.formData)
                    this.Uploadimg()
                } else {
                	this.fileImg = ''
                	console.log('不存在file', file)
                	this.formData = new FormData()
                }
            },
            Uploadimg(){
                this.$confirm('确认上传此图片吗？', '提示', {}).then(() => {
                    const _this= this;
                    _this.$http.post(baseUrl+'/api/attachment/upload', _this.formData, {
                        progress(event) {
                        }
                    })
                        .then(response => {
                            const info = JSON.parse(response.bodyText);
                            // const info = response.body eval('(' + data + ')');
                            const arry = info.data
                            _this.imgArry.push(arry[0])
                            
							_this.url = arry[0].baseUri+arry[0].uri;
							console.log(_this.imgArry)
							_this.clear()
                        }, error => _this.$emit('complete', 500, error.message))
                });
            },
			getlist(){
				const _this = this
				const params = {
					storeId:state.storeId,
					introType:1
				}
				console.log(params)
				$.ajax({
                    type:'POST',
                    dataType:'json',
                    url:baseUrl+"/api/shopRole/selectOne",
                    data:JSON.stringify(params),
                    contentType:'application/json;charset=utf-8',
                    error: function (XMLHttpRequest, textStatus, errorThrown) {},
                    success:function(data){
                    	const info = data.data.shopRoles
                    	console.log(data)
                    	_this.identity= info
                    }
                });
			},
			addIDCard() {
				this.addFormVisible = true
			},
			//新增
			addSubmit: function () {
				const _this = this
				const params = {
					storeId:state.storeId,
					level:this.identity.length+1,
					name:this.orderDetails.name,
					price:this.orderDetails.price,
					icon:this.orderDetails.icon
				}
				console.log(params)
				this.$confirm('确认提交吗？', '提示', {}).then(() => {
					$.ajax({
	                    type:'POST',
	                    dataType:'json',
	                    url:baseUrl+"/api/shopRole/addOne",
	                    data:JSON.stringify(params),
	                    contentType:'application/json;charset=utf-8',
	                    error: function (XMLHttpRequest, textStatus, errorThrown) {},
	                    success:function(data){
	                    	// const info = data.data.shopRoles
	                    	console.log(data)
	                    	// _this.identity= info

	                    	_this.$message({
								message: '提交成功',
								type: 'success'
							});
							_this.addFormVisible = false
							_this.getlist()
	                    }
	                });
						
				});
			},
			// 修改页面显示
			handleEdit(index, row) {
				this.editForm = row
				this.editFormVisible = true
			},
			// 提交修改
			upedit() {
				const _this = this
				const params = {
					id:this.editForm.id,
					price:this.editForm.price
				}
				console.log(params)

				this.$confirm('确认提交吗？', '提示', {}).then(() => {
					$.ajax({
	                    type:'POST',
	                    dataType:'json',
	                    url:baseUrl+"/api/shopRole/updatePrice",
	                    data:JSON.stringify(params),
	                    contentType:'application/json;charset=utf-8',
	                    error: function (XMLHttpRequest, textStatus, errorThrown) {},
	                    success:function(data){
	                    	// const info = data.data.shopRoles
	                    	console.log(data)
	                    	_this.$message({
								message: '提交成功',
								type: 'success'
							});
							_this.editFormVisible = false
	                    }
	                });
						
				});
			},
			shopRoleAdd() {
				const _this = this
				let urls =[]
				for(var i = 0;i<this.imgArry.length;i++){
					urls.push(this.imgArry[i].baseUri+this.imgArry[i].uri)
				}
				const params = {
					storeId:state.id,
					videoUrl:'',
					pictureUrl:urls.toString(),
					introType:0,
					introData:this.desc
				}
				console.log(params)
				this.$confirm('确认发布吗？', '提示', {}).then(() => {
					$.ajax({
	                    type:'POST',
	                    dataType:'json',
	                    url:baseUrl+"/api/shopRole/insertOrUpdate",
	                    data:JSON.stringify(params),
	                    contentType:'application/json;charset=utf-8',
	                    error: function (XMLHttpRequest, textStatus, errorThrown) {},
	                    success:function(data){
	                    	// const info = data.data.shopRoles
	                    	console.log(data)
	                    	_this.$message({
								message: '提交成功',
								type: 'success'
							});
							// _this.editFormVisible = false
	                    }
	                });
						
				});
			}
		},
		mounted() {
			this.getlist();
			console.log(state.id)
		}
	}

</script>

<style>
		
</style>
<template>
	<section>
		<el-col :span="24" style="position: relative;background: #cab78c;height:48px;line-height: 48px;color: #fff;font-size: 16px;padding-left: 20px;margin-bottom: 20px;">
			身份管理</el-col>
		
		<el-col :xs="14" :sm="14" :md="14" :lg="14" style="margin-top: 40px;margin-bottom: 20px">商品图片</el-col>
		<el-col :xs="24" :md="24">
			<!-- 上传图片 -->
			<form style="position: relative;height:234px;">
				<input type="file" style="position:absolute;opacity:0;width:100px;height:30px;margin-right:10px;z-index: 1;"  @change="upload" id="fileInput">
				<el-button type="button" class="el-button el-button--primary el-button--small" style="width:92px;height:30px;background:#9f3333;font-size: 16px;color: #fff;border:1px solid #9f3333; " v-if="this.imgArry.length <3">
					<span >点击上传</span>
				</el-button>
				<span style="font-size: 12px;color: #ababab;margin-left: 10px;">图片格式：jpg,jpeg,png,gif，推荐大小800*800.</span>
				<!-- <el-button type="button" class="el-button el-button--primary el-button--small" :disabled="this.imgArry.length===3" id="btnClear" @click="clear">清空上传</el-button> -->
				<!-- <span style="display: block;font-size: 12px">{{ imageChange }}</span> -->
				<img src="../../assets/upload.png" alt="" style="position: absolute;bottom:0;left:0;z-index: 0;width:160px;height:160px;">
				<div>
					<el-col :xs="6" :md="6" v-for="item in imgArry"  style="margin-top: 42px;position: relative;">
						<img style="position: absolute;right: 54px;top:-17px;" @click="deldetImg(item)" src="../../assets/delet.png">
						<img :src="item" style="width:160px;height: 160px;border: 1px solid #f0f0f0;">
					</el-col>
				</div>

			</form>

		</el-col>


		<!--列表-->
		<el-col :xs="14" :sm="14" :md="14" :lg="14" style="font-size: 16px;color:#616161;margin-bottom: 20px;margin-top: 20px ">会员折扣</el-col>
		<el-table :data="identity" highlight-current-row v-loading="listLoading" style="width: 50%;min-width: 850px;text-align: center;">
			<el-table-column prop="level" label="等级">
			</el-table-column>
			<el-table-column prop="name" label="身份">
			</el-table-column>
			<el-table-column prop="memberDiscount" label="折扣">
			</el-table-column>
			<el-table-column label="操作">
				<template scope="scope">
					<el-button type="text" size="small" @click="handleEdit(scope.$index, scope.row)">修改</el-button>
				</template>
			</el-table-column>
		</el-table>

		<el-col :xs="14" :sm="14" :md="14" :lg="14" style="margin-top: 40px;margin-bottom: 20px;font-size: 16px;">会员折扣介绍</el-col>
		<el-col :span="24">
			<div id = 'editor-trigger' style="height: 500px;"></div>
		</el-col>
		<el-col :xs="14" :sm="14" :md="14" :lg="14" style="margin-top: 40px;margin-bottom: 20px">
		<el-button type="primary" v-on:click="shopRoleAdd">保存</el-button>
		</el-col>
		<!--编辑界面-->
		<el-dialog title="编辑购买折扣" v-model="editFormVisible" :close-on-click-modal="false" >
			<el-form :model="editForm" label-width="160px" :rules="editFormRules" ref="editForm">
				<el-form-item label="角色名称：">
					<el-input v-model="editForm.name" type="text"></el-input>
				</el-form-item>
				<el-form-item label="购买商品折扣：">
					<el-input v-model="editForm.memberDiscount" type="text"></el-input>
				</el-form-item>
			</el-form>
			<div slot="footer" class="dialog-footer" style="text-align: center;">
				<el-button type="primary" @click.native="upedit" :loading="editLoading">确定</el-button>
				<el-button type="primary" @click.native="editFormVisible = false">关闭</el-button>
			</div>
		</el-dialog>
		<!--新增界面-->
		<el-dialog title="新增店铺身份" v-model="addFormVisible" :close-on-click-modal="false" >
			<el-form :model="orderDetails" label-width="160px" :rules="editFormRules" ref="editForm">
				<el-form-item label="角色名称：">
					<el-input v-model="orderDetails.name" type="text" auto-complete="off"></el-input>
				</el-form-item>
				<el-form-item label="价格：">
					<el-input v-model="orderDetails.price" type="text" auto-complete="off"></el-input>
				</el-form-item>
				<el-form-item label="商品数量：" v-if="commissionLine === 3 || commissionLine === 5 || commissionLine === 6">
					<el-input v-model="orderDetails.goodsNum" type="text" auto-complete="off"></el-input>
				</el-form-item>
				<el-form-item label="成品单价：" v-if="commissionLine === 3 || commissionLine === 5 || commissionLine === 6">
					<el-input v-model="orderDetails.costPrice" type="text" auto-complete="off"></el-input>
				</el-form-item>
				<el-form-item label="分佣金额：" v-if="commissionLine === 5 || commissionLine === 6">
					<el-input v-model="commissionPrice" type="text" auto-complete="off"></el-input>
				</el-form-item>
				<el-form-item label="购买商品折扣：" v-if="commissionLine === 6">
					<el-input v-model="memberDiscount" type="text" auto-complete="off"></el-input>
				</el-form-item>
				<el-form-item label="是否可购买：" v-if="commissionLine === 6">
					<el-select v-model="isBuy" placeholder="请选择">
					    <el-option
					      v-for="item in optionsB"
					      :key="item.value"
					      :label="item.label"
					      :value="item.value">
					    </el-option>
					</el-select>
				</el-form-item>
				<el-form-item label="是否可升级：" v-if="commissionLine === 6">
					<el-select v-model="roleInvitedMinNum" placeholder="请选择">
					    <el-option
					      v-for="item in options"
					      :key="item.value"
					      :label="item.label"
					      :value="item.value">
					    </el-option>
					</el-select>
				</el-form-item>
				<el-form-item label="角色图标：">
					<input type="file" style="position:absolute;opacity:0;width:70px;height:30px;margin-right:10px"  @change="upload1" id="fileInput">
				    <el-button type="button" class="el-button el-button--primary el-button--small">
				    	<span >点击上传</span>
				    </el-button>
				    <img style="width: 100px" :src="orderDetails.icon">
					<!-- <el-input v-model="orderDetails.icon" type="text" auto-complete="off"></el-input> -->
				</el-form-item>
				<el-col :span='24'></el-col>
			</el-form>
			<div slot="footer" class="dialog-footer" style="text-align: center;">
				<el-button type="primary" @click.native="addSubmit" :loading="editLoading">确定</el-button>
				<el-button type="primary" @click.native="addFormVisible = false">关闭</el-button>
			</div>
		</el-dialog>
		
	</section>
</template>

<script>
	import { state } from '../../vuex/state'
	import util from '../../common/js/util'
	import {baseUrl , editUser, addUser } from '../../api/api';
	import wangEditor from 'wangEditor'


	export default {
		components: {
            wangEditor
        },
		data() {
			return {
				sjrenum:'',
				isBuy:0,
				roleInvitedMinNum:'',
				commissionPrice:'',
				memberDiscount:'',
				commissionLine:parseInt(localStorage.getItem("commissionLine")),
				addID:'',
				addBtn:false,
				_html:'',
				desc:'',
				radio: '0',
				checked: true,
				identity:[],
				xqsfList:[],
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
				optionsB:[
					{
						value: 0,
		          		label: '可购买'
					},
					{
						value: 1,
		          		label: '不可购买'
					}
				],
				options: [{
		          value: 0,
		          label: '不升级'
		        }, {
		          value: 1,
		          label: '邀请1人升级'
		        }, {
		          value: 2,
		          label: '邀请2人升级'
		        }, {
		          value: 3,
		          label: '邀请3人升级'
		        }, {
		          value: 4,
		          label: '邀请4人升级'
		        }, {
		          value: 5,
		          label: '邀请5人升级'
		        }, {
		          value: 6,
		          label: '邀请6人升级'
		        }, {
		          value: 7,
		          label: '邀请7人升级'
		        }, {
		          value: 8,
		          label: '邀请8人升级'
		        }, {
		          value: 9,
		          label: '邀请9人升级'
		        }, {
		          value: 10,
		          label: '邀请10人升级'
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
					icon:''
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
			// /删除图片
			deldetImg(src){
				this.imgArry.splice(jQuery.inArray(src, this.imgArry),1)
			},
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
            upload1 (event) {
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
                    this.Uploadimg1()
                } else {
                	this.fileImg = ''
                	console.log('不存在file', file)
                	this.formData = new FormData()
                }
            },
            upload3 (event) {
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
                    this.Uploadimg3()
                } else {
                	this.fileImg = ''
                	console.log('不存在file', file)
                	this.formData = new FormData()
                }
            },
            Uploadimg1(){
                // this.$confirm('确认上传此图片吗？', '提示', {}).then(() => {
                    const _this= this;
                    _this.$http.post(baseUrl+'/api/attachment/upload', _this.formData, {
                        progress(event) {
                        }
                    })
                        .then(response => {
                            const info = JSON.parse(response.bodyText);
                            // const info = response.body eval('(' + data + ')');
                            const arry = info.data
                            // _this.imgArry.push(arry[0])
							_this.orderDetails.icon = arry[0].baseUri+arry[0].uri;
							// console.log(_this.imgArry)
							_this.clear()
                        }, error => _this.$emit('complete', 500, error.message))
                // });
            },
            Uploadimg(){
                // this.$confirm('确认上传此图片吗？', '提示', {}).then(() => {
                    const _this= this;
                    _this.$http.post(baseUrl+'/api/attachment/upload', _this.formData, {
                        progress(event) {
                        }
                    })
                        .then(response => {
                            const info = JSON.parse(response.bodyText);
                            // const info = response.body eval('(' + data + ')');
                            const arry = info.data
                            
                            
							_this.url = arry[0].baseUri+arry[0].uri;
							if(_this.imgArry.length <3){
								_this.imgArry.push(arry[0].baseUri+arry[0].uri)
							}
							
							console.log(_this.imgArry)
							_this.clear()
                        }, error => _this.$emit('complete', 500, error.message))
                // });
            },
            Uploadimg3(){
                // this.$confirm('确认上传此图片吗？', '提示', {}).then(() => {
                    const _this= this;
                    _this.$http.post(baseUrl+'/api/attachment/upload', _this.formData, {
                        progress(event) {
                        }
                    })
                        .then(response => {
                            const info = JSON.parse(response.bodyText);
                            // const info = response.body eval('(' + data + ')');
                            const arry = info.data
                            
                            
							_this.editForm.icon = arry[0].baseUri+arry[0].uri;
							// if(_this.imgArry.length <3){
							// 	_this.imgArry.push(arry[0].baseUri+arry[0].uri)
							// }
							
							console.log(_this.imgArry)
							_this.clear()
                        }, error => _this.$emit('complete', 500, error.message))
                // });
            },


            shopRoleAdd() {
				const _this = this
				console.log(this._html)
				console.log(this.see_html)
				if(this._html === '' || this._html === undefined){
					console.log(1)
					this._html = this.see_html
				}else{
					console.log(2)
					this._html = this._html
				}
				const params = {
					pictureUrl:this.imgArry.toString(),
					introData:this._html
				}
				console.log(params)
				$.ajax({
                    type:'POST',
                    dataType:'json',
                    url:baseUrl+"/api/ruleIntroduce/addOrUpdate/member/zone",
                    data:JSON.stringify(params),
                    contentType:'application/json;charset=utf-8',
                    success:function(data){
                    	console.log(data)
                    	if(data.code === 1){
                    		_this.$message({
								message: '保存成功',
								type: 'success'
							});
                    	}else{
                    		_this.$message({
								message: data.msg,
								type: 'error'
							});
                    	}
                    }
                });
			},
			getlist(){
				const _this = this
				const params = {
					storeId:localStorage.getItem("storeId")
				}
				console.log(params)
				$.ajax({
                    type:'POST',
                    dataType:'json',
                    url:baseUrl+"/api/ruleIntroduce/find/member/zone",
                    data:JSON.stringify(params),
                    contentType:'application/json;charset=utf-8',
                    success:function(data){
                    	console.log(data)
                    	if(data.data !== null){
                    		_this.imgArry = data.data.pictureUrl.split(',')
	                    	_this.see_html = data.data.introData
	                    	_this.initEditor()
                    	}else{
	                    	_this.initEditor()
                    	}
                    }
                });
                const paramss = {
					id:localStorage.getItem("storeId")
				}
				$.ajax({
                    type:'POST',
                    dataType:'json',
                    url:baseUrl+"/api/shopRole/selectRuleAll",
                    data:JSON.stringify(paramss),
                    contentType:'application/json;charset=utf-8',
                    success:function(data){
                    	if(data.data === null){
                    		return
                    	}else{
                    		console.log(data)
                    		_this.identity = data.data.shopRoles
                    	}
                    }
                })
			},
			addIDCard(val) {
				console.log(val)
				this.addID = val
				this.addFormVisible = true
			},
			//新增
			addSubmit: function () {
				const _this = this
				const params = {
                    storeId:localStorage.getItem("storeId"),
					level:this.xqsfList.length+1,
					name:this.orderDetails.name,
					price:this.orderDetails.price,
					icon:this.orderDetails.icon,
					
					costPrice:this.orderDetails.costPrice,
					commissionType:0,
					goodsNum:this.orderDetails.goodsNum

				}
				if(this.addID === 2){
					params.level = -100
					params.commissionType = 0
					params.costPrice = this.orderDetails.costPrice
					params.goodsNum = this.orderDetails.goodsNum
				}
				if(this.commissionLine === 5){
					params.commissionPrice = this.commissionPrice
				}
				if(this.commissionLine === 6){
					params.commissionPrice = this.commissionPrice
					params.memberDiscount = this.memberDiscount
					if(this.isBuy === true){
						params.isBuy = 0
					}else{
						params.isBuy = 1
					}
					if(this.roleInvitedMinNum === true){
						params.roleInvitedMinNum = this.sjrenum
					}else{
						params.roleInvitedMinNum = 0
					}
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
	                    	console.log(data)
	                    	if(data.code === 1){
	                    		_this.$message({
									message: '提交成功',
									type: 'success'
								});
								_this.addFormVisible = false
								_this.getlist()
	                    	}else{
	                    		_this.$message({
									message: data.msg,
									type: 'error'
								});
	                    	}
	                    	
	                    }
	                });
						
				});
			},
			// 修改页面显示
			handleEdit(index, row) {
				console.log(row)
				this.editForm = row
				this.editFormVisible = true
			},
			// 提交修改
			upedit() {
				const _this = this
				console.log(this.editForm)
				const params = {
					id:this.editForm.id,
					name:this.editForm.name,
					price:this.editForm.price,
					icon:this.editForm.icon,
					commissionPrice:this.editForm.commissionPrice
				}
				if(this.commissionLine === 3 || this.commissionLine === 5){
					params.goodsNum = this.editForm.goodsNum
					params.costPrice = this.editForm.costPrice
				}
				if(this.commissionLine === 6){
					params.goodsNum = this.editForm.goodsNum
					params.costPrice = this.editForm.costPrice
					params.isBuy = this.editForm.isBuy
					params.roleInvitedMinNum = this.editForm.roleInvitedMinNum
					params.memberDiscount = this.editForm.memberDiscount
				}
				console.log(params)
				this.$confirm('确认提交吗？', '提示', {}).then(() => {
					$.ajax({
	                    type:'POST',
	                    dataType:'json',
	                    url:baseUrl+"/api/shopRole/updateRole",
	                    data:JSON.stringify(params),
	                    contentType:'application/json;charset=utf-8',
	                    error: function (XMLHttpRequest, textStatus, errorThrown) {},
	                    success:function(data){
	                    	// const info = data.data.shopRoles
	                    	console.log(data)
	                    	if(data.code === 1){
		                    	_this.$message({
									message: '提交成功',
									type: 'success'
								});
								_this.editFormVisible = false
	                    	}
	                    }
	                });
						
				});
			},
			
			formaisBuy(row,column){
				if(row.isBuy === 0){
					return '可购买'
				}else{
					return '不可购买'
				}
			},
			formaroleInvitedMinNum(row,column){
				if(row.roleInvitedMinNum === 0){
					return '不升级'
				}else{
					return '邀请'+row.roleInvitedMinNum+'人'
				}
			},
			initEditor(data) {
                const _this = this
                const editor = new wangEditor('editor-trigger')
                editor.config.uploadImgUrl = baseUrl+'/api/attachment/upload'
                editor.config.uploadHeaders = {
                    'contentType' : 'application/json;charset=utf-8'
                }

                editor.config.menus = [
                    'source',
                    '|',
                    'bold',
                    'underline',
                    'italic',
                    'strikethrough',
                    'eraser',
                    'forecolor',
                    'bgcolor',
                    '|',
                    'quote',
                    'fontfamily',
                    'fontsize',
                    'head',
                    'unorderlist',
                    'orderlist',
                    'alignleft',
                    'aligncenter',
                    'alignright',
                    '|',
                    'link',
                    'unlink',
                    'table',
                    // 'emotion',
                    '|',
                    'img',
                    '|',
                    'undo',
                    'redo',
                    'fullscreen'
                ]
                editor.onchange = function () {
                    // 编辑区域内容变化时，实时打印出当前内容
                    _this._html = this.$txt.html()
                    console.log(_this._html);
                }
                editor.create()
                editor.$txt.append(_this.see_html)
            }
		},
		mounted() {
			console.log(state.commissionLine)
			console.log(parseInt(localStorage.getItem("commissionLine")))
			// this.initEditor()
			this.getlist()
		}
	}

</script>

<style>
		
</style>
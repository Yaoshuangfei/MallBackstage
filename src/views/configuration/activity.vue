<template>
	<section>
		<el-col :span="24" style="position: relative;background: #cab78c;height:48px;line-height: 48px;color: #fff;font-size: 16px;padding-left: 20px;margin-bottom: 20px;">
			活动管理
			<el-form :inline="true" style="position: absolute;top:6px;right:0;">
				<el-form-item>
					<!-- <el-button type="primary" v-on:click="addbanner" style="background: transparent;border: transparent;">+  新建活动</el-button> -->
					<router-link :to="{ name: '新建活动'}">
						<el-button style="background: transparent;border: transparent;color: #fff"  type="text">+  新建活动</el-button>
					</router-link>
				</el-form-item>
			</el-form>
		</el-col>
		<!--工具条-->

		<!--列表-->
		<el-table id="bannersss" :data="orderInformation" highlight-current-row v-loading="listLoading" style="width: 100%;text-align: center;">
			<el-table-column prop="activityId" label="活动ID">
			</el-table-column>
			<!-- <el-table-column prop="picture" label="活动图">
				<template scope="scope">
					<img class="img" :src="scope.row.activityLogo" alt="">
				</template>
			</el-table-column> -->
			<el-table-column prop="activityName" label="活动名称">
			</el-table-column>
			<el-table-column prop="activityDetails" label="活动描述">
			</el-table-column>
			<el-table-column prop="activityRulesType" :formatter='isRulesType' label="优惠方式">
			</el-table-column>
			<el-table-column prop="valuationType" :formatter='isaluationType' label="优惠满足方式">
			</el-table-column>
			<el-table-column prop="beginTime" :formatter='beginTimeType' label="活动开始时间">
			</el-table-column>
			<el-table-column prop="endTime" :formatter='endTimeType' label="活动结束时间">
			</el-table-column>
			<el-table-column prop="createTime" :formatter='createTimeType' label="创建时间">
			</el-table-column>
		<!-- 	<el-table-column prop="picture" label="图片">
				<template scope="scope">
					<img class="img" :src="scope.row.picture" alt="">
				</template>
			</el-table-column> -->
			<!-- <el-table-column prop="status"  :formatter='formatterType' label="状态">
			</el-table-column>
			<el-table-column prop="poType"  :formatter='formatterpoType' label="位置">
			</el-table-column> -->
			<!-- <el-table-column prop="createTime" :formatter='formatterTime' label="创建时间" width="300px">
			</el-table-column> -->
			<el-table-column label="操作">
				<template scope="scope">
					<el-button type="text" size="small" @click="handEnabled(scope.row)">修改活动</el-button>
					<el-button type="text" size="small" @click="handDisabled(scope.row)">设置优惠</el-button>
					<el-button type="text" size="small" @click="handmodify(scope.row)">添加商品</el-button>
					<el-button type="text" size="small" @click="deldetBtn(scope.row)">删除</el-button>
					<el-button type="text" size="small" @click="stopBtn(scope.row)">暂停</el-button>
				</template>
			</el-table-column>
		</el-table>

		<!--工具条-->
		<el-col :span="24" class="toolbar" style="background:#fff;">
			<el-pagination layout="prev, pager, next" @current-change="handleCurrentChange" :page-size="10" :total="total" style="float:right;">
			</el-pagination>
		</el-col>
		<!--新增banner-->
		<el-dialog id="addBanner" title="添加banner" v-model="addbannerdiv" :close-on-click-modal="false">
			<el-form :model="uploadDetails" label-width="60px" :rules="editFormRules" ref="editForm" >
				<el-form-item label="链接">
					<el-input v-model="uploadDetails.uploadImgs" type="text" auto-complete="off"></el-input>
				</el-form-item>
				<el-form-item label="banner">
					<input type="file" style="position:absolute;opacity:0;width:70px;height:30px;margin-right:10px"  @change="upload" id="fileInput">
					<button type="button" class="el-button el-button--primary el-button--small">
						<span>点击上传</span>
					</button>
					<button type="button" class="el-button el-button--primary el-button--small" id="btnClear" @click="clear">清空上传</button>
					<span style="display: block;font-size: 12px">{{ imageChange }}</span>
					<!--<button type="button" class="el-button el-button&#45;&#45;primary el-button&#45;&#45;small" id="btnClear" @click="clear">清空上传</button>-->
					<!--<span style="display: block;font-size: 12px">{{ imageChange }}</span>-->
				</el-form-item>
				<el-form-item label="位置">
					<el-select v-model="uploadDetails.poType" placeholder="请选择">
				    <el-option
				      v-for="item in options"
				      :key="item.value"
				      :label="item.label"
				      :value="item.value">
				    </el-option>
				  </el-select>
				</el-form-item>
				<el-form-item label="序号" >
					<el-input v-model="uploadDetails.List" type="text" auto-complete="off"></el-input>
				</el-form-item>
				<el-form-item label="描述">
					<el-input v-model="uploadDetails.information" type="textarea" auto-complete="off" ></el-input>
				</el-form-item>
				<el-col :span='24'></el-col>
			</el-form>
			<div slot="footer" class="dialog-footer" style="text-align: center;">
				<el-button type="primary" @click.native="submitUpload" :loading="editLoading">添加</el-button>
				<el-button type="primary" @click.native="addbannerdiv = false">取消</el-button>
			</div>
		</el-dialog>

		<!--修改banner-->
		<el-dialog title="修改banner" v-model="modifybannerdiv" :close-on-click-modal="false">
			<el-form :model="editForm" label-width="60px" :rules="editFormRules" ref="editForm">
				<el-form-item label="链接">
					<el-input v-model="editForm.link" type="text" auto-complete="off"></el-input>
				</el-form-item>
				<el-form-item label="banner">
					<!-- <el-input v-model="editForm.picture" type="text" auto-complete="off"></el-input> -->
                    <input type="file" style="position:absolute;opacity:0;width:70px;height:30px;margin-right:10px"  @change="modifyload" id="fileInputs">
                    <button type="button" class="el-button el-button--primary el-button--small">
                        <span>点击上传</span>
                    </button>
                    <!-- <button type="button" class="el-button el-button--primary el-button--small" id="btnClears" @click="modifyclear">清空上传</button> -->
                    <!-- <span style="display: block;font-size: 12px">{{ imageChange }}</span> -->
                    <!--<button type="button" class="el-button el-button&#45;&#45;primary el-button&#45;&#45;small" id="btnClear" @click="clear">清空上传</button>-->
                    <!--<span style="display: block;font-size: 12px">{{ imageChange }}</span>-->
                </el-form-item>
				<img style='width: 100px;margin-left: 100px;' :src="editForm.picture">
				<el-form-item label="序号" >
					<el-input v-model="editForm.orderSort" type="text" auto-complete="off"></el-input>
				</el-form-item>
				<el-form-item label="描述">
					<el-input v-model="editForm.desc" type="text" auto-complete="off"></el-input>
				</el-form-item>
				<el-col :span='24'></el-col>
			</el-form>
			<div slot="footer" class="dialog-footer" style="text-align: center;">
				<el-button type="primary" @click.native="modifyUploads" :loading="editLoading">修改</el-button>
				<el-button type="primary" @click.native="modifybannerdiv = false">取消</el-button>
			</div>
		</el-dialog>
	</section>
</template>

<script>
	import { state } from '../../vuex/state'
	import util from '../../common/js/util'
	import {baseUrl} from '../../api/api';

	export default {
		data() {
			return {
				url:'',
				urls:'',
				options: [{
		          value: '1',
		          label: '首页'
		        }, {
		          value: '2',
		          label: '店铺内'
		        }],
				total: 0,
				page: 1,
				listLoading: false,
				editLoading: false,
				editFormRules: {
					name: [
						{ required: true, message: '请输入姓名', trigger: 'blur' }
					]
				},
				//编辑界面数据
				editForm:{},

				addbannerdiv: false,//新增界面是否显示
                modifybannerdiv: false,//新增界面是否显示
				addLoading: false,
				//新增界面数据
				uploadDetails: {
					poType:'',
				},
				//新增界面数据
                modifyDetails: {
				},
				orderInformation:[],
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
//		    清空上传
            clear(){
                let btn = document.getElementById("btnClear");
                let files = document.getElementById("fileInput");
                this.fileImg = '';
                if (files !== null && files.value) {
                    files.value = "";
                    this.formData = new FormData()
                }
            },
		    //图片上传
            upload (event) {
                this.formData = new FormData()
                let file = event.target.files[0]
                const self = this
                if (file) {
                    this.fileImg = file.name
                    this.formData.append('file', file);
                } else {
                    this.fileImg = ''
                    this.formData = new FormData()
                }
            },
            //添加
            submitUpload(){
                this.$confirm('确认添加吗？', '提示', {}).then(() => {
                    const _this= this;
                    _this.$http.post(baseUrl+'/api/attachment/upload', _this.formData, {
                        progress(event) {
                        }
                    })
                        .then(response => {
                            const info = JSON.parse(response.bodyText);
							_this.url = info.data[0].baseUri+info.data[0].uri;
                            _this.UploadImg();
                        }, error => _this.$emit('complete', 500, error.message))
                });
            },
//			图片上传ajax
			UploadImg(){
                const _this= this;
                const params = {
                    link:this.uploadDetails.uploadImgs,
                    picture:this.url,
                    orderSort:this.uploadDetails.List,
                    poType:this.uploadDetails.poType,
                    desc:this.uploadDetails.information,
                };
                var url = baseUrl+"/api/indexAdvert/add";
                var data =JSON.stringify(params);
                $.ajax({
                    type:'POST',
                    dataType:'json',
                    url:url,
                    data:data,
                    contentType:'application/json;charset=utf-8',
                    error: function (XMLHttpRequest, textStatus, errorThrown) {},
                    success:function(data){
                        if(!data.success){
                            alert(data.msg)
						}else{
                            _this.addbannerdiv = false;
                            _this.getlist();
						}
                    }
                });


			},



			getlist(){
				const _this = this
				const params = {
                    pageNum:this.page,
                    size:10,
                    storeId:localStorage.getItem("storeId"),
                    status:'',
                    activityId:'',
                    activityName:''
				}
                $.ajax({
                    type:'POST',
                    dataType:'json',
                    url:baseUrl+'/api/activity/selectList',
                    data:JSON.stringify(params),
                    contentType:'application/json;charset=utf-8',
                    success:function(data){
                        console.log(data)
                        const info = data.data
                        _this.total = info.total
                        _this.orderInformation  = info.list
                    }
                })
			},
			// 删除
			deldetBtn(row){
				console.log(row)
				const _this = this
				const params = {
					activityId:row.activityId,
					activityStatus:2
				}
				$.ajax({
                    type:'POST',
                    dataType:'json',
                    url:baseUrl+'/api/activity/updateStatus',
                    data:JSON.stringify(params),
                    contentType:'application/json;charset=utf-8',
                    success:function(data){
                        console.log(data)
                        _this.getlist()
                    }
                })
			},
			// 暂停
			stopBtn(row){
				const _this = this
				const params = {
					activityId:row.activityId,
					activityStatus:1
				}
				$.ajax({
                    type:'POST',
                    dataType:'json',
                    url:baseUrl+'/api/activity/updateStatus',
                    data:JSON.stringify(params),
                    contentType:'application/json;charset=utf-8',
                    success:function(data){
                        console.log(data)
                        _this.getlist()
                    }
                })
			},
			handleCurrentChange(val) {
				this.page = val;
				this.getlist();
			},
			isRulesType (row,column){
                let curTime = row.activityRulesType
                if(curTime === 1){
                	return "满就送"
                }else if(curTime === 2){
                	return "满就减"
                }else if(curTime === 3){
                	return "限时打折"
                }
            },
            isaluationType (row,column){
                let curTime = row.valuationType
                if(curTime === 0){
                	return "按数量"
                }else if(curTime === 1){
                	return "按金额"
                }
            },
            beginTimeType (row,column){
                let curTime = row.beginTime
                if(curTime !== '' || curTime !== null){
	                curTime = new Date(curTime).toLocaleString()
	                return curTime
                }
            },
            endTimeType (row,column){
                let curTime = row.endTime
                if(curTime !== '' || curTime !== null){
	                curTime = new Date(curTime).toLocaleString()
	                return curTime
                }
            },
            createTimeType(row,column){
                let curTime = row.createTime
                if(curTime !== '' || curTime !== null){
	                curTime = new Date(curTime).toLocaleString()
	                return curTime
                }
            },






























			//删除
            handleEdit: function (index, row) {
				this.$confirm('确认删除该记录吗?', '提示', {
					type: 'warning'
				}).then(() => {
                    const _this= this;
                    const params = {
                        id:row.id
                    };
                    var url = baseUrl+"/api/indexAdvert/delete/one";
                    var data =JSON.stringify(params);
                    $.ajax({
                        type:'POST',
                        dataType:'json',
                        url:url,
                        data:data,
                        contentType:'application/json;charset=utf-8',
                        error: function (XMLHttpRequest, textStatus, errorThrown) {},
                        success:function(data){
                            if(!data.success){
                                alert(data.msg)
                            }else{
                                _this.$message({
                                    message: '删除成功',
                                    type: 'success'
                                });
                                _this.getlist();
                            }
                        }
                    });
//					this.listLoading = true;
					//NProgress.start();
//					let para = { id: row.id };
//					removeUser(para).then((res) => {
//						this.listLoading = false;
//						//NProgress.done();
//						this.$message({
//							message: '删除成功',
//							type: 'success'
//						});
//						this.getUsers();
//					});
				}).catch(() => {

				});
			},
            //启用
            handEnabled: function (index, row) {
                this.$confirm('确认启用该广告位吗?', '提示', {
                    type: 'warning'
                }).then(() => {
                    const _this= this;
                    const params = {
                        id:row.id
                    };
                    var url = baseUrl+"/api/indexAdvert/enable";
                    var data =JSON.stringify(params);
                    $.ajax({
                        type:'POST',
                        dataType:'json',
                        url:url,
                        data:data,
                        contentType:'application/json;charset=utf-8',
                        error: function (XMLHttpRequest, textStatus, errorThrown) {},
                        success:function(data){
                            if(!data.success){
                                alert(data.msg)
                            }else{
                                _this.$message({
                                    message: '启用成功',
                                    type: 'success'
                                });
                                _this.getlist();
                            }
                        }
                    });
//					this.listLoading = true;
                    //NProgress.start();
//					let para = { id: row.id };
//					removeUser(para).then((res) => {
//						this.listLoading = false;
//						//NProgress.done();
//						this.$message({
//							message: '删除成功',
//							type: 'success'
//						});
//						this.getUsers();
//					});
                }).catch(() => {

                });
            },
            //禁用
            handDisabled: function (index, row) {
                this.$confirm('确认禁用该广告位吗?', '提示', {
                    type: 'warning'
                }).then(() => {
                    const _this= this;
                    const params = {
                        id:row.id
                    };
                    var url = baseUrl+"/api/indexAdvert/disable";
                    var data =JSON.stringify(params);
                    $.ajax({
                        type:'POST',
                        dataType:'json',
                        url:url,
                        data:data,
                        contentType:'application/json;charset=utf-8',
                        error: function (XMLHttpRequest, textStatus, errorThrown) {},
                        success:function(data){
                            if(!data.success){
                                alert(data.msg)
                            }else{
                                _this.$message({
                                    message: '禁用成功',
                                    type: 'success'
                                });
                                _this.getlist();
                            }
                        }
                    });
//					this.listLoading = true;
                    //NProgress.start();
//					let para = { id: row.id };
//					removeUser(para).then((res) => {
//						this.listLoading = false;
//						//NProgress.done();
//						this.$message({
//							message: '删除成功',
//							type: 'success'
//						});
//						this.getUsers();
//					});
                }).catch(() => {

                });
            },

//			修改
            handmodify: function (index, row) {
			    this.modifybannerdiv = true;
			    this.editForm = row;
            },


//            清空上传
            modifyclear(){
        let btn = document.getElementById("btnClears");
        let files = document.getElementById("fileInputs");
        this.fileImg = '';
        if (files !== null && files.value) {
            files.value = "";
            this.formData = new FormData()
        }
    },
    //图片上传后修改
            modifyload (event) {
                this.formData = new FormData()
                let file = event.target.files[0]
                const self = this
                if (file) {
                    this.fileImg = file.name
                    this.formData.append('file', file);
                    this.modifyUpload()
                } else {
                    this.fileImg = ''
                    this.formData = new FormData()
                }
            },
    //修改
            modifyUpload(){
                this.$confirm('确认修改吗？', '提示', {}).then(() => {
                    const _this= this;
                    _this.$http.post(baseUrl+'/api/attachment/upload', _this.formData, {
                        progress(event) {
                        }
                    })
                        .then(response => {
                            const info = JSON.parse(response.bodyText);
                            _this.editForm.picture = info.data[0].baseUri+info.data[0].uri;
                            // _this.modifyUploads();
                        }, error => _this.$emit('complete', 500, error.message))
                });
    },
    //			图片上传ajax
            modifyUploads(){
        const _this= this;
        const params = {
            id:this.editForm.id,
            link:this.editForm.link,
            picture:this.editForm.picture,
            orderSort:this.editForm.orderSort,
            desc:this.editForm.desc,
        };
        var url = baseUrl+"/api/indexAdvert/update";
        var data =JSON.stringify(params);
        $.ajax({
            type:'POST',
            dataType:'json',
            url:url,
            data:data,
            contentType:'application/json;charset=utf-8',
            error: function (XMLHttpRequest, textStatus, errorThrown) {},
            success:function(data){
                if(!data.success){
                    alert(data.msg)
                }else{
                    _this.modifybannerdiv = false;
                    _this.getlist();
                }
            }
        });


    },
			//显示添加banner页面
			addbanner: function (index, row) {
				this.addbannerdiv = true;
				// this.uploadDetails = Object.assign({}, row);
			},
			//编辑

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
			},
            formatterTime(row,column){
                let curTime = row.createTime;
                curTime = new Date(curTime).toLocaleString()
                return curTime
            },
            formatterType(row,column){
                let type = ''
                if(row.status === 1){
                	type = '启用'
                }else{
                	type = '禁用'
                }
                return type
            },
            formatterpoType(row,column) {
            	return row.poType === 1 ?'首页':'店铺内'
            }
		},
		mounted() {
			 this.getlist();
		}
	}
</script>

<style>
	.el-dialog--small {
    	width: 600px;
    	border-radius: 10px
	}
	.el-form-item__label{
		text-align: left;
	}
	.el-dialog__body{
		margin-left: 0 ! important ;
	}
	.img {
		width:80px;
		height:80px;
	}
	#addBanner .el-dialog__title {font-size: 16px;font-weight: inherit;color: #cab78c;}
	#addBanner .el-dialog__header {color: #cab78c;text-align: center;border-bottom: 1px dashed #cab78c;padding-bottom: 10px;}
	#addBanner .el-dialog__headerbtn {display: none;}
	#addBanner .el-dialog__body {width:380px;margin: auto !important;}
	#addBanner textarea {height:120px;resize: none;}
	#addBanner .dialog-footer button {margin: 0 50px 20px 50px;width:92px;height:32px;}
	#bannersss .el-table th>.cell {text-align: center;background: #f4f2e8;}
	#bannersss .el-table td, .el-table th.is-leaf {background: #f4f2e8;}
	#bannersss .el-table__fixed-header-wrapper thead div, .el-table__header-wrapper thead div {background: #f4f2e8;color: #616161;}
</style>
<template>
	<section>
		<!--列表-->
		<el-table :data="orderInformation" highlight-current-row v-loading="listLoading" style="width: 100%;margin-top: 40px">
			<el-table-column type="index" label="序号">
			</el-table-column>
			<el-table-column label="类型">
				<template scope="scope">视频</template>
			</el-table-column>
			<el-table-column label="金额">
				<template scope="scope">888</template>
			</el-table-column>
			<el-table-column prop="createTime" :formatter='createTimes' label="付款时间">
			</el-table-column>
			<el-table-column prop="timeOutDate" :formatter='timeOutTime' label="到期时间">
			</el-table-column>
			<el-table-column label="操作">
				<template scope="scope">
					<!-- <el-button type="text" size="small">查看</el-button> -->
					<!-- <el-button type="text" size="small" @click="seeBtn(scope.row)">查看</el-button> -->
					<el-button type="text" size="small" @click="handleEdit(scope.row)">续费</el-button>
				</template>
			</el-table-column>
		</el-table>

		<!--工具条-->
		<el-col :span="24" class="toolbar" style="background:#fff;">
			<el-pagination layout="prev, pager, next" @current-change="handleCurrentChange" :page-size="10" :total="total" style="float:right;">
			</el-pagination>
		</el-col>

		<!--编辑界面-->
		<el-dialog title="订单详情" v-model="editFormVisible" :close-on-click-modal="false" >
			<el-form :model="orderDetails" label-width="160px">
				<el-form-item label="商品编号">
					<div>{{orderDetails.goodsId }}</div>
				</el-form-item>
				<el-form-item label="数量：">
					<div>{{orderDetails.orderTotal }}</div>
				</el-form-item>
				<el-form-item label="评价：">
					<div>{{orderDetails.content }}</div>
				</el-form-item>
				<el-col :span='24'></el-col>
			</el-form>
			<div slot="footer" class="dialog-footer" style="text-align: center;">
				<el-button type="primary" @click.native="editFormVisible = false">关闭</el-button>
			</div>
		</el-dialog>
		<!-- 回复 -->
		<el-dialog title="支付密码" v-model="hfInfo" :close-on-click-modal="false" >
			<el-form label-width="80px">
				<el-form-item label="支付密码">
					<el-input type="password" auto-complete="off" v-model="pasword"></el-input>
				</el-form-item>
			</el-form>
			<div slot="footer" class="dialog-footer" style="text-align: center;">
				<el-button type="primary" @click.native="editSubmit" :loading="editLoading">保存</el-button>
				<el-button type="primary" @click.native="hfInfo = false">关闭</el-button>
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
				hfInfo:false,
				pasword:'',
				formData: new FormData(),
				CommodityPictures:[],
				selectSubjectStatus: [{
					value:1,
					label:'好评'
				},{
					value:2,
					label:'中评'
				},{
					value:3,
					label:'差评'
				}],
				options: [{
		          value: '1',
		          label: '商品编号'
		        }, {
		          value: '2',
		          label: '手机号'
		        }, {
		          value: '3',
		          label: '昵称'
		        }, {
		          value: '4',
		          label: '真实姓名'
		        }],
				filters: {
					name: '',
					status:'',
					type:''
				},
				total: 1,
				page: 1,
				listLoading: false,
				editFormVisible: false,//编辑界面是否显示
				editLoading: false,
				//编辑界面数据
				editForm: {
				},
				deco:'',
				addFormVisible: false,//新增界面是否显示
				addLoading: false,
				//新增界面数据
				orderDetails: {
					coreUser:{},
					goods:{}
				},
				orderInformation:[]
			}
		},
		methods: {
			getlist(){
                const _this = this
                this.orderInformation = []
                $.ajax({
                    type:'POST',
                    dataType:'json',
                    url:baseUrl+"/api/store/storeVideo/selectOne",
                    data:JSON.stringify({}),
                    contentType:'application/json;charset=utf-8',
                    success:function(data){
                      _this.orderInformation.push(data.data)
                    }
                });   
            },
			// 查看
			seeBtn(row){
				this.editFormVisible = true
				this.orderDetails = row
			},
			// 回复
			handleEdit(row){
				this.hfInfo = true
				this.editForm = row
			},
			editSubmit(){
				const _this = this
                const params = {
                    payPassword:this.pasword
                }
                $.ajax({
                    type:'POST',
                    dataType:'json',
                    url:baseUrl+"/api/store/storeVideo/pay",
                    data:JSON.stringify(params),
                    contentType:'application/json;charset=utf-8',
                    error: function (XMLHttpRequest, textStatus, errorThrown) {},
                    success:function(data){
                      if(data.code === 1){
                        _this.$message({
                          message: '成功',
                          type: 'success'
                        });
                        _this.passIval = false
                        _this.baonian = false
                      }else{
                        _this.$message({
                          message: data.msg,
                          type: 'error'
                        });
                      }
                    }
                });
			},
			// /删除图片
	        deldetImg(src){
	          this.CommodityPictures.splice(jQuery.inArray(src, this.CommodityPictures),1)
	        },
			handleCurrentChange(val) {
				this.page = val;
				this.getlist();
			},
			createTimes(row,column){
                let curTime = row.createTime;
                curTime = new Date(curTime).toLocaleString()
                return curTime
            },
			timeOutTime(row, column){
				let curTime = row.timeOutDate;
                curTime = new Date(curTime).toLocaleString()
                return curTime
			}
		},
		mounted() {
			this.getlist();
		}
	}

</script>

<style>
	
</style>
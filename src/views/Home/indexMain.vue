<template>
	<section>
		<!--顶部数据-->
		<el-row :gutter="20" class="main_top">
			<el-col :span="6">
				<div class="grid-content bg-purple Finance">
					<p>会员人数</p>
					<p>{{memberCount}}</p>
					<p>
						<span>{{memberPercentage}}%</span>
						<span>来自上周</span>
					</p>
				</div>
			</el-col>
			<el-col :span="6">
				<div class="grid-content bg-purple Finance">
					<p>总营业额</p>
					<p>{{sumTotalMoney}}</p>
					<p>
						<span>{{moneyPercentage}}%</span>
						<span>来自上月</span>
					</p>
				</div>
			</el-col>
			<el-col :span="6">
				<div class="grid-content bg-purple Finance">
					<p>点击量</p>
					<p>{{visitCount}}</p>
					<p>
						<span>{{visitPercentage}}%</span>
						<span>来自今天</span>
					</p>
				</div>
			</el-col>
			<el-col :span="6">
				<div class="grid-content bg-purple Finance">
					<p>成交量</p>
					<p>{{sumTotalQuantity}}</p>
					<p>
						<span>{{quantityPercentage}}%</span>
						<span>来自上周</span>
					</p>
				</div>
			</el-col>
		</el-row>
		<!--统计图-->
		<el-row class="statistics">
			<el-col :span="24" class="statistics_title">
				<div class="grid-content bg-purple">
					<div style="float:left" class="statistics_title_left">
						<span>财务报表</span>
						<!--<span>{{baobiaoName}}</span>-->
					</div>
					<div style="float:right;margin-right: 2%;" class="statistics_title_right">
						<span class="wrapper">
							  <el-radio-group v-model="radio" class="wrapper_btn" @change = "click" style="position: relative;top:-5px;">
							  	<el-radio-button  v-for="item in ruleAll" :label="item.id">{{item.name}}</el-radio-button>
							  </el-radio-group>
  						</span>
					</div>
				</div>
			</el-col>
			<el-col :span="24" class="statistics_img">
				<div class="grid-content bg-purple-light">
					<el-row>
						<el-col :span="16">
							<div id="chartColumn" style="width:100%; height:480px;"></div>
						</el-col>
						<el-col :span="8">
							<el-col :span="24" style="text-align: center;font-weight: 900;font-size: 24px;">店铺收入支出分析图</el-col>
							<div id="chartPie" style="width:100%; height:380px;margin-top: 50px;"></div>
						</el-col>
					</el-row>
				</div>
			</el-col>
		</el-row>
		<el-row class="statistics_bottom">
			<el-col :span="8" style="margin-top: 0;">
				<div class="grid-content bg-purple" >
					<div class="statistics_bottom_left_top">待办事项</div>
					<div class="statistics_bottom_main">
						<ul >
							<li style="cursor: pointer;" v-for="item in daibanList"><a :title="item.notifyContent">{{item.notifyContent}}</a></li>
						</ul>
					</div>
					<el-col :span="24" class="toolbar" style="background:#fff;text-align: center;margin-top: 80px;">
						<el-pagination layout="prev, pager, next" @current-change="handleCurrentChange" :page-size="10" :total="total">
						</el-pagination>
					</el-col>
				</div>
			</el-col>
			<el-col :span="15" style="float:right;margin-top: 0;overflow-y: auto;">
				<div class="grid-content bg-purple">
					<div class="statistics_bottom_left_top">待发货订单</div>
					<div class="statistics_bottom_main">
						<ul>
							<li style="cursor: pointer;" v-for="item in dingdanList"><a :title="item.notifyContent">{{item.notifyContent}} ---{{item.notifyTitle}}</a></li>
						</ul>
					</div>
					<el-col :span="24" class="toolbar" id="toolbarasd" style="background:#fff;text-align: center;margin-top: 80px;">
						<el-pagination layout="prev, pager, next" @current-change="handleCurrentChanges" :page-size="10" :total="totals">
						</el-pagination>
					</el-col>
				</div>
			</el-col>
		</el-row>
		<el-dialog :title="addTitle" v-model="editFormVisible" :close-on-click-modal="false" :show-close='false'>
			<el-col :span="24" :offset='10' v-if="remarkInfo !== '' ">未通过信息：{{remarkInfo}}</el-col>
			<el-form :model="sfinfo" label-width="180px" style="margin-left: 40px;margin-top: 40px" :rules="rules" ref="sfinfo">
              <el-form-item label="公司名称">
                <el-input v-model="sfinfo.companyName"></el-input>
              </el-form-item>
		      <el-form-item label="法人姓名">
		        <el-input v-model="sfinfo.realName"></el-input>
		      </el-form-item>
		      <el-form-item label="法人身份证号" prop="legalCardCode">
		        <el-input v-model="sfinfo.legalCardCode" ></el-input>
		      </el-form-item>
		      <el-form-item label="手机号">
		        <el-input v-model="sfinfo.storeMobile"></el-input>
		      </el-form-item>
		      <el-form-item label="银行名称">
		        <el-input v-model="sfinfo.bankName"></el-input>
		      </el-form-item>
		      <el-form-item label="对公账户账号">
		        <el-input v-model="sfinfo.bankCode"></el-input>
		      </el-form-item>
		      <el-form-item label="运营地址">
		        <el-input v-model="sfinfo.theAddress"></el-input>
		      </el-form-item>
		      <el-col :span="24" :offset="8" v-if="frImgurl !== '' "><img style="width: 100px" :src="frImgurl"></el-col>
		      <el-form-item label="上传法人身份证">
		      	<input type="file" style="position:absolute;opacity:0;width:70px;height:30px;margin-right:10px"  @change="upload(1)" id="fileInput">
				<button type="button" class="el-button el-button--primary el-button--small">
					<span>点击上传</span>
				</button>
		      </el-form-item>
		      <el-col :span="24" :offset="8" v-if="frImgurlPointer !== '' "><img style="width: 100px" :src="frImgurlPointer"></el-col>
		      <el-form-item label="上传法人手持身份证">
		      	<input type="file" style="position:absolute;opacity:0;width:70px;height:30px;margin-right:10px"  @change="upload(2)" id="fileInput">
				<button type="button" class="el-button el-button--primary el-button--small">
					<span>点击上传</span>
				</button>
		      </el-form-item>
		      <el-col :span="24" :offset="8" v-if="businessImgurl !== '' "><img style="width: 100px" :src="businessImgurl"></el-col>
		      <el-form-item label="上传营业执照">
		      	<input type="file" style="position:absolute;opacity:0;width:70px;height:30px;margin-right:10px"  @change="upload(3)" id="fileInput">
				<button type="button" class="el-button el-button--primary el-button--small">
					<span>点击上传</span>
				</button> 
		      </el-form-item>
		      <el-col :span="24" :offset="8" v-if="bankImgurl !== '' "><img style="width: 100px" :src="bankImgurl"></el-col>
		      <el-form-item label="上传银行开户许可证">
		      	<input type="file" style="position:absolute;opacity:0;width:70px;height:30px;margin-right:10px"  @change="upload(4)" id="fileInput">
				<button type="button" class="el-button el-button--primary el-button--small">
					<span>点击上传</span>
				</button>
		      </el-form-item>
		  </el-form>
			<div slot="footer" class="dialog-footer" style="text-align: center;">
				<el-button type="primary" @click.native="clupLoad">提交</el-button>
			</div>
		</el-dialog>
		<el-dialog title="提示" v-model="dashVisible" :close-on-click-modal="false" :show-close='false'>
			<el-col style="margin-bottom: 20px;" :offset='4'><h1>管理员正在审核中，请耐心等待。谢谢配合！</h1></el-col>
		</el-dialog>
	</section>
</template>
<script>
    import echarts from 'echarts'
    import { state } from '../../vuex/state'
	import {baseUrl} from '../../api/api';
    export default {
        data() {
            return {
                baobiaoName:'日营业额',
            	addTitle:'添加身份信息',
            	frImgurl:'',
            	frImgurlPointer:'',
            	businessImgurl:'',
            	bankImgurl:'',
            	formData: new FormData(),
            	rules:{
            		legalCardCode: [
				        { required: true, message: '请输入身份证号', trigger: 'blur' },
				        { pattern: /(^[1-9]\d{5}(18|19|([23]\d))\d{2}((0[1-9])|(10|11|12))(([0-2][1-9])|10|20|30|31)\d{3}[0-9Xx]$)|(^[1-9]\d{5}\d{2}((0[1-9])|(10|11|12))(([0-2][1-9])|10|20|30|31)\d{2}$)/, message: '证件号码格式有误！', trigger: 'blur' }
				      ]
            	},
            	sfinfo:{companyName:''},
                total:7,
                page: 1,
                totals:10,
                pages: 1,
            	editFormVisible:false,//上传材料
            	dashVisible:false,//待审核
            	remarkInfo:'',//未通过信息
            	sfinfoId:'',//修改身份信息ID
            	daibanList:[],
            	dingdanList:[],
                radio: 0,
                ruleAll:[{
                	name:'日报表',
                	id:0
                },{
                	name:'周报表',
                	id:1
                },{
                	name:'月报表',
                	id:2
                },{
                	name:'季报表',
                	id:3
                },{
                	name:'年报表',
                	id:4
                }],
                chartColumn: null,
                chartBar: null,
                chartLine: null,
                chartPie: null,
                chartmap: null,
                visitPercentage:'',
                visitCount:'',
                sumTotalMoney:'',
                moneyPercentage:'',
                memberCount:'',
                memberPercentage:'',
                sumTotalQuantity:'',
                quantityPercentage:'',
                type:0,

                listAll:[],//线图
                sj:[],
                cj:[],
                sp:[],
                pj:[],
                parobj:[],//饼图
                parName:[]
            }
        },
        methods: {
        	//图片上传
            upload (index) {
                this.formData = new FormData()
                let file = event.target.files[0]
                const self = this
                if (file) {
                    this.fileImg = file.name
                    this.formData.append('file', file);
                    this.submitUpload(index)
                } else {
                    this.fileImg = ''
                    this.formData = new FormData()
                }
            },
            submitUpload(index){
                    const _this= this;
                    _this.$http.post(baseUrl+'/api/attachment/upload', _this.formData, {
                        progress(event) {
                        }
                    })
                        .then(response => {
                            const info = JSON.parse(response.bodyText);
                            if(index === 1){
                            	_this.frImgurl = info.data[0].baseUri+info.data[0].uri;
                            }else if(index === 2){
                            	_this.frImgurlPointer = info.data[0].baseUri+info.data[0].uri;
                            }else if(index === 3){
                            	_this.businessImgurl = info.data[0].baseUri+info.data[0].uri;
                            }else if(index === 4){
                            	_this.bankImgurl = info.data[0].baseUri+info.data[0].uri;
                            } 
                        }, error => _this.$emit('complete', 500, error.message))
            },
            clupLoad(){
            	const _this = this
                console.log(this.sfinfo.companyName)
            	const params = {
            		realName:this.sfinfo.realName,
            		legalCardCode:this.sfinfo.legalCardCode,
            		storeMobile:this.sfinfo.storeMobile,
            		bankName:this.sfinfo.bankName,
            		bankCode:this.sfinfo.bankCode,
            		theAddress:this.sfinfo.theAddress,
            		cardImgF:this.frImgurl,
            		cardImgW:this.frImgurlPointer,
            		businessLicense:this.businessImgurl,
            		bankImgW:this.bankImgurl,
                    companyName:this.sfinfo.companyName
            	}
            	let url = ''
                console.log(this.sfinfoId)
            	if(this.sfinfoId === ''){
            		url = '/api/coreUspAuthentication/add'
            	}else{
            		url = '/api/coreUspAuthentication/updateDetails'
            		params.id = this.sfinfoId
            	}
                console.log(url)
        		$.ajax({
                    type:'POST',
                    dataType:'json',
                    url:baseUrl+url,
                    data:JSON.stringify(params),
                    contentType:'application/json;charset=utf-8',
                    success:function(data){
                    	if(data.code === 1){
                    		_this.editFormVisible = false
                            _this.$router.push({ path: '/login' });
                    	}else{
                    		_this.$message.error(data.msg);
                    	}
                    }
                });
            },
        	getGroup(){
        		const _this = this;
                const params = {
                    pageNum :this.page,
                    size:7,
                    notifyTypes:'1,5,6,10'
                }
        		$.ajax({
                    type:'POST',
                    dataType:'json',
                    url:baseUrl+"/api/notify/selectListGroup",
                    data:JSON.stringify(params),
                    contentType:'application/json;charset=utf-8',
                    success:function(data){
                        console.log(data)
                        if(data.code === 1){
                        	const info = data.data
                        	_this.daibanList = info.records;
                            _this.total = info.total
                        }
                    }
                });
        	},
        	getDDWL(){
                const _this = this;
                const params = {
                    pageNum :this.pages,
                    size:7,
                    notifyTypes:'2'
                }
                $.ajax({
                    type:'POST',
                    dataType:'json',
                    url:baseUrl+"/api/notify/selectListGroup",
                    data:JSON.stringify(params),
                    contentType:'application/json;charset=utf-8',
                    success:function(data){
                        console.log(data)
                        if(data.code === 1){
                            const info = data.data
    						if(info.records.length===0){
    							$('#toolbarasd').hide();
    						}else {
                                _this.dingdanList = info.records;
                                _this.totals = info.total
    						}
                        }

                    }
                });
        	},
        	getlist(){
        		const _this = this
				$.ajax({
	              type:'POST',
	              dataType:'json',
	              url:baseUrl+"/api/admin/userCashFlow/data/analysis",
	              data:{},
	              contentType:'application/json;charset=utf-8',
	              success:function(data){
	                  console.log(data)
	                const info = data.data
	                _this.visitPercentage = (info.visitPercentage*100).toFixed(6)
	                _this.visitCount = info.visitCount
	                _this.sumTotalMoney = info.sumTotalMoney
	                _this.moneyPercentage = (info.moneyPercentage*100).toFixed(6)
	                _this.memberCount = info.memberCount
	                _this.memberPercentage = (info.memberPercentage*100).toFixed(6)
	                _this.sumTotalQuantity = info.sumTotalQuantity
	                _this.quantityPercentage = (info.quantityPercentage*100).toFixed(6)
	              }
	          });
        	},
        	click(val) {
        		this.type = val
                if(val === 0){
                    this.baobiaoName = '日营业额'
                }else if(val === 1){
                    this.baobiaoName = '周营业额'
                }else if(val === 2){
                    this.baobiaoName = '月营业额'
                }else if(val === 3){
                    this.baobiaoName = '季营业额'
                }else if(val === 4){
                    this.baobiaoName = '年营业额'
                }
        		this.getline()
			},
        	getline(){
        		const _this = this
        		_this.sj = []
        		_this.cj = []
        		_this.sp = []
        		_this.pj = []
        		_this.parobj = []//初始化饼图数据
        		_this.parName = []
        		_this.listAll = []//初始化线形图数据
        		const params = {
        			type:this.type,
                    storeId:localStorage.getItem("storeId")
        		}
        		
        		$.ajax({
	              type:'POST',
	              dataType:'json',
	              url:baseUrl+"/api/orderMall/selectByPayTimeGroup",
	              data:JSON.stringify(params),
	              contentType:'application/json;charset=utf-8',
	              success:function(data){
	                const info = data.data
	                const linelist = info.analysisVOList
	                for(var i = 0;i<linelist.length;i++){
	                	// 时间
                        var date=new Date(linelist[i].payTime);
                        if(_this.type === 0){
                             _this.sj.push(date.getHours()+'时')
                        }else if(_this.type === 1 || _this.type === 3){
                             _this.sj.push((date.getMonth()+1)+"月"+date.getDate()+'日')
                        }else if(_this.type === 2 ){

                            _this.sj.push(date.getDate()+'日')
                        }else{
                            _this.sj.push(date.getFullYear()+"-"+(date.getMonth()+1))
                        }
	                	_this.cj.push(linelist[i].moneyAll)
	                	_this.sp.push(linelist[i].countAll)
	                	_this.pj.push(linelist[i].avgPrice)
	                }
	                const obj = {}
                	obj.name = '成交额总数'
                	obj.type = 'line'
                	obj.smooth = true
                	obj.data = _this.cj
            		_this.listAll.push(obj)

            		const obj1 = {}
                	obj1.name = '成交商品总数'
                	obj1.type = 'line'
                	obj1.smooth = true
                	obj1.data = _this.sp
            		_this.listAll.push(obj1)

            		const obj2 = {}
                	obj2.name = '成交额平均值'
                	obj2.type = 'line'
                	obj2.smooth = true
                	obj2.data = _this.pj
            		_this.listAll.push(obj2)
            		



					//饼图数据
	                const ordlist= info.orderMalls
	                for(var i = 0;i<ordlist.length;i++){
	                	const obj = {}
	                	if(ordlist[i].orderType === 3){
	                		obj.name = '业务充值'
	                	}else if(ordlist[i].orderType === 4){
	                		obj.name = '余额充值'
	                	}else if(ordlist[i].orderType === 5){
	                		obj.name = '商品购买'
	                	}else if(ordlist[i].orderType === 6){
	                		obj.name = '店铺身份购买'
	                	}else if(ordlist[i].orderType === 7){
	                		obj.name = '平台身份购买 '
	                	}else if(ordlist[i].orderType === 8){
	                		obj.name = '补货'
	                	}else if(ordlist[i].orderType === 9){
	                		obj.name = '金豆充值'
	                	}else if(ordlist[i].orderType === 13){
	                		obj.name = '便付劵充值'
	                	}else{
	                		obj.name = '业务充值'
	                	}
	                	obj.value = ordlist[i].totalMoney
	                	_this.parName.push(obj.name)
	                	_this.parobj.push(obj)
	                }
	                _this.drawColumnChart()
	                _this.drawPieChart()
	              }
	          });
        	},
            drawColumnChart() {
                this.chartColumn = echarts.init(document.getElementById('chartColumn'));
                this.chartColumn.setOption({
                    title: { text: this.baobiaoName },
                    tooltip: {},
                    legend: {
                        data:['成交额总数','成交商品总数','成交额平均值']
                    },
                    xAxis: {
                        type : 'category',
                        boundaryGap : false,
                        data: this.sj
                    },
                    yAxis: {
                        type : 'value'
					},
                    series:this.listAll
                });
            },
            drawPieChart() {
                this.chartPie = echarts.init(document.getElementById('chartPie'));
                this.chartPie.setOption({
                    title: {
//                        text: '店铺收入支出分析图',
                        x: 'center'
                    },
                    tooltip: {
                        trigger: 'item',
                        formatter: "{a} <br/>{b} : {c} ({d}%)"
                    },
                    legend: {
                        orient: 'vertical',
                        left: 'left',
						data:this.parName
                    },
                    series: [
                        {
                            name: '访问来源',
                            type: 'pie',
                            radius: '55%',
                            center: ['50%', '60%'],
                            data:this.parobj,
                            itemStyle: {
                                emphasis: {
                                    shadowBlur: 10,
                                    shadowOffsetX: 0,
                                    shadowColor: 'rgba(0, 0, 0, 0.5)'
                                }
                            }
                        }
                    ]
                });
            },
            formatterTime(row){
                return  new Date(row).toLocaleString()
            },
            getSHinfo(){
            	const _this = this
            	$.ajax({
	              type:'POST',
	              dataType:'json',
	              url:baseUrl+"/api/coreUspAuthentication/checkDetails",
	              data:JSON.stringify({}),
	              contentType:'application/json;charset=utf-8',
	              success:function(data){
                    console.log(data)
	                const info = data.data
                    if(info === null){

                    }else{
    	                _this.remarkInfo = info.remark
    	                _this.sfinfoId = info.id
                    }
	              }
	          });
            },
            handleCurrentChange(val) {
                this.page = val;
                this.getGroup();
            },
            handleCurrentChanges(val) {
                this.pages = val;
                this.getDDWL();
            },
        },
        mounted: function () {
            console.log(state.storeStatus)
        	if(state.storeStatus === 4){
        		this.dashVisible = true 
        	}else if(state.storeStatus === 0){//待审核
        		this.editFormVisible = true
        	}else if(state.storeStatus === 2){//审核未通过
        		this.addTitle = '修改身份信息'
        		this.getSHinfo()
        	}
        	this.getlist();
        	this.getline();
        	this.getGroup();
        	this.getDDWL();
        },
        // updated: function () {
        //     this.drawCharts()
        // }
    }
</script>

<style scoped lang="scss">
	.main_top {
		text-align: center;
		padding: 40px 0 35px 0;
		font-family: "Microsoft YaHei";
		.Finance {
			p {
				margin: 0;
				padding: 0;
				color: #6a7c8f;
			}
			p:nth-child(1) {  font-size: 12px;color: #616161;  }
			p:nth-child(2) {  font-size: 28px;color: #9f3333; }
			p:nth-child(3) {
				font-size: 12px;
				color: #616161;
				span:nth-child(1){
					color: #cab78c;
				}
			}
		}
		.el-row {
			margin-bottom: 20px;
			&:last-child {
				margin-bottom: 0;
			}
		}
		.el-col {
			border-radius: 4px;
		}
		.bg-purple-dark {
			/*background: #99a9bf;*/
		}
		.bg-purple {
			/*background: #d3dce6;*/
		}
		.bg-purple-light {
			/*background: #e5e9f2;*/
		}
		.grid-content {
			border-radius: 4px;
			min-height: 36px;
		}
		.row-bg {
			padding: 10px 0;
			background-color: #f9fafc;
		}
	}
	.statistics {
		font-family: "Microsoft YaHei";
		border: 1px solid #e6eef9;
		.statistics_img , .statistics_title {
			margin-top: 0;
		}
		.statistics_title {
			height:60px;
			border-bottom: 1px dashed #f4f2e8;
			line-height: 60px;
			.statistics_title_left {
				height:60px;
				span:nth-child(1) {
					font-size: 20px;
					color: #cab78c;
					margin: 0 20px;
				}
				span:nth-child(2) {
					font-size: 18px;
					color: #6a7c8f;
				}
			}
			.statistics_title_right {
				height:70px;
			}
		}
	}
	.statistics_bottom {
		font-family: "Microsoft YaHei";
		margin-top: 30px;
		height:454px;
		.grid-content {
			border: 1px solid #e6eef9;
			height:454px;
			.statistics_bottom_left_top {
				height:70px;
				font-size: 20px;
				font-weight: 900;
				color: #cab78c;
				padding: 0 5%;
				border-bottom: 1px solid #cdcdcd;
				line-height: 70px;
			}
			.statistics_bottom_main {
				ul ,li {
					list-style-type: none;
					margin: 0;
					padding: 0;
				}
				ul {
					width:90%;
					margin: auto;
				}
				li {
					height:34px;
					line-height: 34px;
					font-size: 14px;
					color: #333;
					white-space:nowrap;
					text-overflow:ellipsis;
					-o-text-overflow:ellipsis;
					overflow: hidden;
					width:100%;
				}
			}
		}
	}
</style>
<style>
	/*.el-radio-button:first-child .el-radio-button__inner {border: none;}
	.el-radio-button:first-child .el-radio-button__inner {border-radius: 10px;}
	.el-radio-button:last-child .el-radio-button__inner {border-radius: 10px;}
	.el-radio-button__orig-radio:checked+.el-radio-button__inner {background-color:#22ac38;}
	.el-radio-button__inner {background: #f7f7f7;font-size: 18px;color: #6a7c8f;}
	.el-radio-button__inner:hover {color: #22ac38;}
	.wrapper_btn span {
		border: transparent;
		border-radius: 10px;
		margin: 0 10px;
	}*/
</style>
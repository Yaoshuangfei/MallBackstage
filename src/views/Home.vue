<template>
	<el-row class="container">
		<el-col :span="24" id="main_top">
			<div id="main_top_name">
				<span>{{sysUserName}}</span>，欢迎您登录商家后台管理中心！
				<el-dropdown trigger="hover" style="float:right;">
					<el-dropdown-item divided @click.native="logout" style="border-top: inherit;margin-top: inherit;">退出</el-dropdown-item>
					<!--<el-dropdown-menu slot="dropdown">-->
						<!--<el-dropdown-item>我的消息</el-dropdown-item>-->
						<!--<el-dropdown-item>设置</el-dropdown-item>-->
						<!--<el-dropdown-item divided @click.native="logout">退出登录</el-dropdown-item>-->
					<!--</el-dropdown-menu>-->
				</el-dropdown>
			</div>
		</el-col>
		<el-col :span="24" class="header" >
			<div id="main_title">
				<el-col>
					<router-link :to="{ name: '主页'}">
						<span><img src="http://resources.51att.cn/ATTACHMENT/ATTACHMENT/1bccc3cf-8d44-4482-84e1-82d84d56e25c.png" alt=""></span>
						<span>商家后台管理中心</span>
					</router-link>
				</el-col>
			</div>
			<!--<el-col :span="10" class="logo" :class="collapsed?'logo-collapse-width':'logo-width'">-->
				<!--<router-link :to="{ name: '主页'}">-->
						<!--<el-button style="font-size: 20px" type="primary">{{sysName}}</el-button>-->
				<!--</router-link>-->
			<!--</el-col>-->
			<!--<el-col :span="10">-->
				<!--<div class="tools" @click.prevent="collapse">-->
					<!--<i class="fa fa-align-justify"></i>-->
				<!--</div>-->
			<!--</el-col>-->
		</el-col>
		<div id="main_conter"  class="main">
			<el-col :span="24"  style="margin: 40px 0 ;">
				<el-col :span="6">
					<aside :class="collapsed?'menu-collapsed':'menu-expanded'">
						<!--导航菜单-->
						<!-- <el-menu :default-active="$route.path" class="el-menu-vertical-demo" @open="handleopen" @close="handleclose" @select="handleselect"
                             unique-opened router v-show="!collapsed">
                            <template v-for="(item,index) in meunList" >
                                <el-submenu>
                                    <template slot="title"><i :class="item.iconCls"></i>{{item.name}}</template>
                                    <el-menu-item v-for="child in item.children" v-if="!child.hidden">{{child.name}}</el-menu-item>
                                </el-submenu>
                                <el-menu-item v-if="item.leaf&&item.children.length>0" ><i :class="item.iconCls"></i>{{item.children[0].name}}</el-menu-item>
                            </template>
                        </el-menu> -->
						<el-menu :default-active="$route.path" class="el-menu-vertical-demo" @open="handleopen" @close="handleclose" @select="handleselect"
								 unique-opened router v-show="!collapsed">
							<template  v-for="(item,index) in arry"  v-if="!item.hidden">
								<el-submenu :index="index+''" v-if="!item.leaf">
									<template slot="title"><i :class="item.iconCls"></i>{{item.name}}</template>
									<el-menu-item v-for="child in item.children" :index="child.path" v-if="!child.hidden">{{child.name}}</el-menu-item>
								</el-submenu>
								<el-menu-item v-if="item.leaf&&item.children.length>0" :index="item.children[0].path"><i :class="item.iconCls"></i>{{item.children[0].name}}</el-menu-item>
							</template>
						</el-menu>
						<!--导航菜单-折叠后-->
						<ul class="el-menu el-menu-vertical-demo collapsed" v-show="collapsed" ref="menuCollapsed">
							<li v-for="(item,index) in $router.options.routes" v-if="!item.hidden" class="el-submenu item">
								<template v-if="!item.leaf">
									<div class="el-submenu__title" style="padding-left: 20px;" @mouseover="showMenu(index,true)" @mouseout="showMenu(index,false)"><i :class="item.iconCls"></i></div>
									<ul class="el-menu submenu" :class="'submenu-hook-'+index" @mouseover="showMenu(index,true)" @mouseout="showMenu(index,false)">
										<li v-for="child in item.children" v-if="!child.hidden" class="el-menu-item" style="padding-left: 40px;" :class="$route.path==child.path?'is-active':''" @click="$router.push(child.path)">{{child.name}}</li>
									</ul>
								</template>
								<template v-else>
									<ul>
										<li class="el-submenu">
											<div class="el-submenu__title el-menu-item"
												 style="padding-left: 20px;height: 56px;line-height: 56px;padding: 0 20px;" :class="$route.path==item.children[0].path?'is-active':''"
												 @click="$router.push(item.children[0].path)"><i :class="item.iconCls"></i></div>
										</li>
									</ul>
								</template>
							</li>
						</ul>
					</aside>
				</el-col>
				<el-col :span="18">
					<section class="content-container">
						<div class="grid-content bg-purple-light">
							<!--<el-col :span="24" class="breadcrumb-container">-->
								<!--<strong class="title">{{$route.name}}</strong>-->
								<!--<el-breadcrumb separator="/" class="breadcrumb-inner">-->
									<!--<el-breadcrumb-item v-for="item in $route.matched">-->
										<!--{{ item.name }}-->
									<!--</el-breadcrumb-item>-->
								<!--</el-breadcrumb>-->
							<!--</el-col>-->
							<el-col :span="24" class="content-wrapper">
								<transition name="fade" mode="out-in">
									<router-view></router-view>
								</transition>
							</el-col>
						</div>
					</section>
				</el-col>
			</el-col>
		</div>
		<!-- <el-col :span="24" class="footer">
			<el-row>
				<el-col :span="24">11111</el-col>
			</el-row>
		</el-col> -->
		<el-col :span="24" id="bottom">
			<div id="bottomConter">
				<div class="bottomConterText">
					<div style="margin-right:10px;"><img src="../assets/icon_address.png" alt=""></div>
					<div><div style="font-size: 18px;">地址</div>
						<div style="font-size: 14px;">浙江省杭州市拱墅区天堂E谷</div></div>
				</div>
				<div class="bottomConterText">
					<div style="margin-right:10px;width:100px;height:100px;">
						<img src="../assets/xyl.png">
					</div>
					<div>
						<div style="font-size: 18px;text-align: left;">扫描二维码</div>
						<div style="font-size: 14px;text-align: left;">下载“吸引力商城APP”</div>
					</div>
				</div>
				<div class="bottomConterText">
					<div style="margin-right: :10px;"><img src="../assets/icon_phone.png" alt=""></div>
					<div>
						<div style="font-size: 18px;text-align: left;">电话</div>
						<div style="font-size: 14px;text-align: left;">暂未开通</div>
					</div>
				</div>
			</div>
			<div id="bottomLink">
				<ul>
					<li><a href="#">关于我们</a></li>
					<li><a href="#">常见问题</a></li>
					<li><a href="#">友情链接</a></li>
					<li><a href="#">法律声明</a></li>
					<li><a href="#">联系我们</a></li>
					<li><a href="#">操作指引</a></li>
					<li><a href="#">地址导航</a></li>
				</ul>
				<p style="margin-top: 40px;">Copyright 2017杭州边缘网络科技有限公司  浙CP备45678945</p>
			</div>
		</el-col>
	</el-row>
</template>

<script>
	import { state } from '../vuex/state';
    import {baseUrl } from '../api/api';
	export default {
		data() {
			return {
                imgUrl:'',
				sysName:'商家后台管理',
				arry:[],
				collapsed:false,
				sysUserName: '',
				sysUserAvatar: '',
				form: {
					name: '',
					region: '',
					date1: '',
					date2: '',
					delivery: false,
					type: [],
					resource: '',
					desc: ''
				},
				meunList: [],
				list:[{
					name:'xiangmu1'
				},{
					name:'xiangmu1'
				},{
					name:'xiangmu1'
				},{
					name:'xiangmu1'
				}]
			}
		},
		methods: {
			// 菜单获取
			getMenuList() {
				const _this = this
				const params = {
		              accountId: '1',
		              accessToken: '1'
		        }
			   	$.post("http://192.168.10.18:8080/shangfu-admin-web//backstage/menu/getMenu",
			    { param: JSON.stringify(params) },
			        function(data){
			            const info = eval('(' + data + ')');
			            const resepene = JSON.parse(info)
			            const list = resepene.obj
			            for(var i = 0;i<list.length;i++){
			            	var father = {}
			            	father.name = list[i].name
			            	if(list[i].nodes.length !==0){
		                		var chlid = []
		                		father.children = chlid
		                		_this.menunodes(list[i].nodes,chlid);
		                		_this.meunList.push(father)
		                	}else{
		                		_this.meunList.push(father)
		                	}
			            }
			        }
			    );
			},
			menunodes(list,chlid){
				for(var x = 0;x<list.length;x++){
					var childrens = {}
					childrens.name = list[x].name
					if(list[x].nodes.length !== 0){
						var chlids = []
						childrens.children = chlids
						this.menunodes(list[x].nodes,chlids);
					}
					chlid.push(childrens)
				}
			},
			onSubmit() {
			},
			handleopen() {
			},
			handleclose() {
			},
			handleselect: function (a, b) {
			},
			//退出登录
			logout: function () {
				var _this = this;
				this.$confirm('确认退出吗?', '提示', {
					//type: 'warning'
				}).then(() => {
					sessionStorage.removeItem('user');
					_this.$router.push('/login');
				}).catch(() => {

				});


			},
			//折叠导航栏
			collapse:function(){
				this.collapsed=!this.collapsed;
			},
			showMenu(i,status){
				this.$refs.menuCollapsed.getElementsByClassName('submenu-hook-'+i)[0].style.display=status?'block':'none';
			},
            getName(){
                const _this = this
                $.ajax({
                    type:'POST',
                    dataType:'json',
                    url:baseUrl+"/api/core/selectLoginUser",
                    data:{},
                    contentType:'application/json;charset=utf-8',
                    success:function(data){
                        console.log(data.data)
						_this.sysUserName = data.data.nickName;
                    }
                });
			}
		},
		mounted() {
		    this.getName();
			var user = sessionStorage.getItem('user');
			if (user) {
				user = JSON.parse(user);
				this.sysUserName = user.name || '';
				this.sysUserAvatar = user.avatar || '';
			}
			this.arry = this.$router.options.routes
			if(state.commissionLine !== 3 && state.commissionLine !== 5){
				for(var i = 0;i<this.arry[5].children.length;i++){
					if(this.arry[5].children[i].name === '分销规则管理'){
						this.arry[5].children.splice(i,1)
					}
				}
				state.keys = true
			}else if(state.commissionLine === 3 || state.commissionLine === 5){
				if(state.keys){
					const obj = {}
					obj.name = '分销规则管理'
					obj.path = "/rule"
					this.arry[5].children.push(obj)
				}
				state.keys = false
			}
		}
	}

</script>

<style scoped lang="scss">
	@import '~scss_vars';
	.container {
		position: absolute;
		background: #fff;
		top: 0px;
		bottom: 0px;
		width: 100%;
		.header {
			height: 160px;
			line-height: 60px;
			background:url(../assets/main_top_bg.jpg) no-repeat top center;
			color:#fff;
			.userinfo {
				text-align: right;
				padding-right: 35px;
				float: right;
				.userinfo-inner {
					cursor: pointer;
					color:#fff;
					img {
						width: 40px;
						height: 40px;
						border-radius: 20px;
						margin: 10px 0px 10px 10px;
						float: right;
					}
				}
			}
			.logo {
				//width:230px;
				height:60px;
				font-size: 22px;
				padding-left:20px;
				padding-right:20px;
				border-color: rgba(238,241,146,0.3);
				border-right-width: 1px;
				border-right-style: solid;
				img {
					width: 40px;
					float: left;
					margin: 10px 10px 10px 18px;
				}
				.txt {
					color:#fff;
				}
			}
			.logo-width{
				width:230px;
			}
			.logo-collapse-width{
				width:60px
			}
			.tools{
				padding: 0px 23px;
				width:14px;
				height: 60px;
				line-height: 60px;
				cursor: pointer;
			}
		}
		.footer {

		}
		.main {
			/*display: flex;*/
			/*!*background: #324057;*!*/
			/*position: absolute;*/
			/*top: 208px;*/
			/*bottom: 0px;*/
			/*overflow: hidden;*/
			aside {
				flex:0 0 318px;
				width: 318px;
				/*// position: absolute;*/
				/*// top: 0px;*/
				/*// bottom: 0px;*/
				.el-menu{
					height: 100%;
				}
				.collapsed{
					width:60px;
					.item{
						position: relative;
					}
					.submenu{
						position:absolute;
						top:0px;
						left:60px;
						z-index:99999;
						height:auto;
						display:none;
					}

				}
			}
			.menu-collapsed{
				flex:0 0 60px;
				width: 60px;
				overflow-y: auto;
			}
			.menu-expanded{
				flex:0 0 318px;
				width: 318px;
				overflow-y: auto;
			}
			.content-container {
				/*// background: #f1f2f7;*/
				flex:1;
				/*// position: absolute;*/
				/*// right: 0px;*/
				/*// top: 0px;*/
				/*// bottom: 0px;*/
				/*// left: 230px;*/
				padding-left: 20px;
				.breadcrumb-container {
					//margin-bottom: 15px;
					.title {
						width: 200px;
						float: left;
						color: #475669;
					}
					.breadcrumb-inner {
						float: right;
					}
				}
				.content-wrapper {
					background-color: #fff;
					box-sizing: border-box;
				}
			}
		}
	}
</style>
<style>
	#main_top {  height:48px;line-height: 48px;background: #fff;  }
	#main_top_name {width:1280px;position: relative;margin: auto;text-align: center;color: #616161;font-size: 14px;}
	#main_top_name>span {color: #9f3333;}
	#main_title {line-height: 160px;font-size:40px;color: #cab78c;width:1280px;position: relative;margin: auto; }
	#main_title a {color: #cab78c;text-decoration:none;}
	#main_title img {width: 80px;  height: 80px;  border-radius: 50%;vertical-align: middle;border: 1px solid #cab78c;}
	#main_conter {    width: 1280px;  position: relative;  margin: 40px auto;}
	#main_conter .el-menu {background: inherit;}
	#main_conter .menu-expanded>.el-menu {border: 1px solid #e6eef9;}
	#main_conter .menu-expanded>.el-menu>.el-menu-item, .el-submenu__title {border-bottom: 1px solid #e6eef9;height:92px;line-height: 92px;}
	#main_conter .menu-expanded>.el-menu .el-menu-item, .el-submenu__title {border-bottom: 1px solid #e6eef9;}
	#main_conter .menu-expanded>.el-menu ul {background: #f4f2e8;}
	#main_conter .menu-expanded>.el-menu li .el-menu{color: #616161;}
	#main_conter .menu-expanded>.el-menu li .el-menu--horizontal.el-menu--dark .el-submenu .el-menu-item.is-active, .el-menu-item.is-active{color: #cab78c;}
	#main_conter .menu-expanded>.el-menu li .el-submenu .el-menu-item:hover, .el-submenu__title:hover{background: #f4f2e8;color: #cab78c;}
	#main_conter .menu-expanded .el-submenu.is-active .el-submenu__title {border-bottom-color: #cab78c;}
	#main_conter .menu-expanded .el-submenu .el-submenu__title {font-size: 24px;color: #616161;}
	#main_conter .menu-expanded .el-submenu.is-opened .el-submenu__title {color: #cab78c;}
	#main_conter .menu-expanded>.el-menu>.el-menu-item {font-size: 24px;color: #616161;}
	#bottom {width:100%;height:340px;background: #181f29;position: relative;margin: auto;color: #fff;}
	#bottom #bottomConter ,#bottom #bottomLink {width:850px;position: relative;margin:60px auto;text-align: center;}
	#bottomConter>.bottomConterText {display: inline-block;}
	#bottomConter>.bottomConterText:nth-child(1) {text-align: left;width:250px;}
	#bottomConter>.bottomConterText:nth-child(2) {text-align: center;width:400px;}
	#bottomConter>.bottomConterText:nth-child(3) {text-align: right;width:192px;}
	#bottomConter>.bottomConterText>div {display: inline-block;}
	#bottom #bottomLink a {color: #fff;text-decoration: none;}
	#bottom #bottomLink li {list-style-type: none;display: inline-block;width:86px;text-align: center;border-right: 2px solid #fff;}
	#bottom #bottomLink li:last-child {border-right:none;}
</style>
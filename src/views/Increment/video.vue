<template>
  <section>
      <el-col :span="24" style="position: relative;background: #cab78c;height:48px;line-height: 48px;color: #fff;font-size: 16px;padding-left: 20px;margin-bottom: 20px;">
          视频管理
      </el-col>
    <el-form  label-width="100px" style="margin-left: 40px;">
      <el-form-item label="展示位置">
        <el-select v-model="value" placeholder="请选择">
            <el-option v-for="item in options" :key="item.value" :label="item.label" :value="item.value"></el-option>
        </el-select>
      </el-form-item>
      <el-form-item  style="position: relative;">
        <div style="color: #ababab;font-size: 12px;margin-left: 10px;">视频格式为avi/mp4等，大小不超过50M</div>
            <div style="position: absolute;top:0;left:-100px;">
                <input type="file" style="position:absolute;opacity:0;width:70px;height:30px;margin-right:10px"  @change="upload" id="fileInput">
                <el-button type="button" class="el-button el-button--primary el-button--small" style="width:94px;height:32px;background: #9f3333;border: 1px solid #9f3333;cursor: pointer;">
                    <span >点击上传</span>
                </el-button>
            </div>
      </el-form-item>
      <el-col :span="24" style="padding-bottom: 20px;;height:160px;">
          <!--<el-col><img src="../../assets/upload.png" alt=""></el-col>-->
        <el-col :span="3"  style="margin-right: 60px;width:260px;height:160px;background-image: url(../../assets/upload.png);position: relative;">
            <img src="../../assets/upload.png" alt="" style="position: absolute;top:0;left:0;z-index: 0;">
          <video v-if="url !== '' "  width="260" height="160" controls="controls" style="position: absolute;top:0;left:0;z-index: 1;">
              <source :src="url" type="video/mp4" />
          </video>
        </el-col>
      </el-col>
      <el-button type="primary" @click="uploadBtn" style="width:80px;height:32px;background: #cab78c;border: 1px solid #cab78c;cursor: pointer;margin-top: 20px;">确定</el-button>
      <el-button type="primary" @click="clearBtn" style="width:80px;height:32px;background: #fff;color: #333;border: 1px solid #cab78c;cursor: pointer;margin-top: 20px;margin-left: 100px;">取消</el-button>
      <el-col :span="24" style="padding-bottom: 20px;margin-top: 20px;color: #616161;font-size: 16px;">历史上传的视频 :</el-col>
        <el-col :span="24" style="padding-bottom: 20px;">
        <el-col :span="8"  style="margin-right: 60px" v-for="item in urlArry">
          <!--<el-col :span="5" :offset="12" v-if="item.index === 1">店铺</el-col>-->
          <!--<el-col :span="5" :offset="12" v-if="item.index === 2">身份</el-col>-->
          <video v-if="urlArry !== [] "  width="260" height="160" controls="controls"><!-- autoplay="autoplay" 直接播放 -->
              <source :src="item.url" type="video/mp4" />
          </video>
          <el-button type="text" @click="onSubmit(item.index,item.url)" style="width:60px;height:32px;border: 1px solid #cab78c;border-radius: 20px;line-height: 12px;background:#cab78c;color: #fff; ">上传</el-button>
        </el-col>
      </el-col>
      
  </el-form>

  <el-col v-if="baonian"  :span="24" style="width: 100%;height:100%;position: fixed;top:0;left:0;z-index:9;background: rgba(0,0,0,.5);text-align:center;">
        <el-col id="goumai" :span="8" style="height: 216px;width: 520px;background: #fff;border-radius: 5px;position: relative;top:50%;margin-top: -150px;left:50%;margin-left: -300px;">
            <el-col :span="2" id="close" @click.native="close">X</el-col>
            <el-col :span="24" style="height:46px;line-height: 46px;border-bottom: 1px dashed #cab78c;font-size: 16px;color: #cab78c;">提示</el-col>
            <el-col :span="24" style="text-align: center;font-size: 16px;color:#616161;margin: 40px 0; ">是否使用上传视频功能</el-col>
            <el-col :span="24">
                <el-col :span="12"><el-button :disabled="shiyong"  @click="oneBtn" style="background:#cab78c;color: #fff;border: 1px solid #cab78c;">试用一个月</el-button></el-col>
                <el-col :span="12" ><el-button  @click="storeVideo" style="background:#fff;color: #cab78c;border: 1px solid #cab78c;">包年888元</el-button></el-col>
            </el-col>
        </el-col>
        <el-col v-if="passIval" :span="24" style="left:50%;height: 150px;width: 410px;background: #eee;border-radius: 5px;position: fixed;z-index: 10;transform: translateX(-50%);top:30%;">
            <el-col :span="24" style="margin-top: 40px;margin-left: 70px">
                <el-col :span="5">支付密码：</el-col>
                <el-col :span="10"><el-input type="password" v-model="pasword"> </el-input></el-col>
            </el-col>
            <el-col :span="5" :offset="8" style="margin-top: 20px;">
                <el-button type="primary" @click="onPassUp">确认</el-button>
            </el-col>
            <el-col :span="5" style="margin-top: 20px;">
                <el-button type="primary" @click="clertBtn">取消</el-button>
            </el-col>
        </el-col>
  </el-col>
  </section>
</template>

<script>
  import { state } from '../../vuex/state'
  import util from '../../common/js/util'
  import {baseUrl  } from '../../api/api';

  export default {
    data() {
      return {
        shiyong:false,
        historyUrl:'',
        passIval:false,
        listLoading:true,
        baonian:true,
        value:'',
        pasword:'',
        formData: new FormData(),
        url:'',
        urlArry:[],
        options:[{
            value:1,
            label:'店铺内'
        },{
            value:2,
            label:'身份详情banner'
        }]
      }
    },
    methods: {
        close(){
            window.history.go(-1);
        },
            // 视频上传返回url
              upload (event) {
                this.formData = new FormData()
                        let file = event.target.files[0]
                        console.log(this.formData)
                        console.log(file)
                        const self = this
                        if (file) {
                          console.log('存在file', file)
                            this.formData.append('file', file)
                            console.log(this.formData)
                            this.Uploadimg()
                        } else {
                          console.log('不存在file', file)
                          this.formData = new FormData()
                        }
                },
                Uploadimg(){
                    const _this= this;
                    _this.$http.post(baseUrl+'/api/attachment/upload', _this.formData, {
                        progress(event) {
                        }
                    })
                        .then(response => {
                            const info = JSON.parse(response.bodyText);
                            const arry = info.data
                            _this.url = arry[0].baseUri+arry[0].uri;
                        }, error => _this.$emit('complete', 500, error.message))
            },
        clearBtn(){
            let btn = document.getElementById("btnClear");
            let files = document.getElementById("fileInput");
            this.url = '';
            if (files !== null && files.value) {
                files.value = "";
                this.formData = new FormData()
            }
        },
            // 上传视频
            uploadBtn(){
                const _this = this
                const params = {
                    index:this.value,
                    url:this.url
                }
                console.log(params)
                $.ajax({
                    type:'POST',
                    dataType:'json',
                    url:baseUrl+"/api/store/storeVideo/upload",
                    data:JSON.stringify(params),
                    contentType:'application/json;charset=utf-8',
                    error: function (XMLHttpRequest, textStatus, errorThrown) {},
                    success:function(data){
                      if(data.code === 1){
                          _this.gethistory()
                      }else{
                        _this.$message({
                          message: data.msg,
                          type: 'error'
                        })
                      }
                    }
                });
            },
            onSubmit(index,url){
              const _this = this
                const params = {
                    index:index,
                    url:url
                }
                $.ajax({
                    type:'POST',
                    dataType:'json',
                    url:baseUrl+"/api/store/storeVideo/upload",
                    data:JSON.stringify(params),
                    contentType:'application/json;charset=utf-8',
                    error: function (XMLHttpRequest, textStatus, errorThrown) {},
                    success:function(data){
                      if(data.code === 1){
                          _this.gethistory()
                      }else if(data.code === 4){
                          _this.baonian = true
                          _this.shiyong = true
                      }else{
                        _this.$message({
                          message: data.msg,
                          type: 'error'
                        })
                      }
                    }
                });
            },
            // 使用一个月
            oneBtn(){
                const _this = this
                $.ajax({
                    type:'POST',
                    dataType:'json',
                    url:baseUrl+"/api/store/storeVideo/addCheck",
                    data:JSON.stringify({}),
                    contentType:'application/json;charset=utf-8',
                    error: function (XMLHttpRequest, textStatus, errorThrown) {},
                    success:function(data){
                      if(data.code === 1){
                        _this.baonian = false
                      }
                    }
                });   
                
            },
            // 购买视频
            storeVideo(){
                this.passIval = true;
                $('#goumai').hide();
//                this.baonian = false;
            },
            clertBtn(){
                this.passIval = false;
                $('#goumai').show();
            },
            //确认密码
            onPassUp(){
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
            // 获取视频信息
            getlist(){
                const _this = this
                $.ajax({
                    type:'POST',
                    dataType:'json',
                    url:baseUrl+"/api/store/storeVideo/check",
                    data:JSON.stringify({}),
                    contentType:'application/json;charset=utf-8',
                    success:function(data){
                      if(data.code === 2){
                        _this.baonian = false
                      }else if(data.code === 4){
                        _this.baonian = true
                        _this.shiyong = true
                      }else{
                        _this.baonian = true
                      }
                    }
                });   
            },
             // 获取展示的视频信息
            gethistory(){
                const _this = this
                const params = {
                  pageNum:1,
                  pageSize:5
                }
                $.ajax({
                    type:'POST',
                    dataType:'json',
                    url:baseUrl+"/api/store/storeVideo/selectList",
                    data:JSON.stringify(params),
                    contentType:'application/json;charset=utf-8',
                    error: function (XMLHttpRequest, textStatus, errorThrown) {},
                    success:function(data){
                      _this.urlArry = data.data.list
                      _this.historyUrl = data.data.list[0].url
                    }
                });   
            }
        },
    mounted() {
      this.getlist();
      this.gethistory()
    }
  }

</script>

<style>
  
  .el-form-item__label{
    text-align: left;
  }
    #close {position: absolute;top:10px;right:0;color: #bfcbd9;cursor: pointer;}
    #close:hover {color: rgb(202, 183, 140);}
</style>
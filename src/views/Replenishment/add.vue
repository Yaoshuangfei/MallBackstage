<template>
  <section>
    <el-col :span="24" style="position: relative;background: #cab78c;height:48px;line-height: 48px;color: #fff;font-size: 16px;padding-left: 20px;">
      店铺样式管理
    </el-col>
    <el-col :span="24">
      <el-col :span="4" style="padding-bottom: 20px;margin-top: 20px;font-size: 20px;color: #cab78c;">补货的商品</el-col>
      <el-col :span="2" :offset="8"  style="padding-bottom: 20px;margin-top: 20px;font-size: 18px;">
        <el-button type="text" @click.native="shopAdd">添加</el-button>
      </el-col>
    </el-col>
    <el-table :data="shopTable" highlight-current-row v-loading="listLoading">
      <el-table-column type="index" label="序号" width="239px">
      </el-table-column>
      <el-table-column prop="name" label="商品名称" width="350px">
      </el-table-column>
      <el-table-column prop="price" label="售价" width="350px">
        <template scope="scope">
          <el-input type="text" class="noneborder" v-model="scope.row.price"></el-input>
        </template>
      </el-table-column>
    </el-table>
    <el-col :span="20" :offset="4" style="margin-top: 40px">
      <el-col :span="4"><el-button type="primary">修改</el-button></el-col>
      <el-button type="primary" @click="Preservation">保存</el-button>
    </el-col>
    <el-col :span="24" style="margin-top: 40px;border-bottom: 1px dashed #cab78c;"></el-col>
    <el-col :soan="24" style="margin-top: 20px;font-size: 18px;">
      <el-col :span="2">混批设置</el-col>
    </el-col>
    <el-col :span="24" style="margin-top: 20px;margin-bottom: 20px">
      <template>
        <el-radio-group v-model="radio" @change="radiochange">
            <el-radio :label="2">总数量</el-radio>
            <el-radio :label="3">总价格</el-radio>
        </el-radio-group>
      </template>
    </el-col>
    <el-table :data="table" border highlight-current-row v-loading="listLoading">
      <el-table-column type="index" label="序号" width="239px">
      </el-table-column>
      <el-table-column prop="name" label="身份" width="350px">
      </el-table-column>
      <el-table-column prop="addPrice" :label="tableName" width="350px">
        <template scope="scope">
            <el-input type="text" class="noneborder" v-model="scope.row.addPrice"></el-input>
        </template>
      </el-table-column>
    </el-table>

    <el-col :span="24" :offset="6" style="margin-top: 40px">
      <!-- <el-col :span="4"><el-button type="primary">修改</el-button></el-col> -->
      <el-button type="primary" @click="Preservation">保存</el-button>
    </el-col>
    <el-col :span="24" style="margin-top: 40px;border-bottom: 1px dashed #cab78c;"></el-col>
    <!-- <el-col :span="2" style="padding-bottom: 20px;margin-top: 20px;font-size: 18px;">删除</el-col> -->
    <el-col :span="24" style="padding-top: 20px;font-size: 18px;margin-bottom: 20px;">商品图片</el-col>
    <el-form :model="filters" label-width="180px" style="margin-left: 40px;margin-top: 40px">
      
      <el-col :span="24">
            <el-col :span="24" >
            <input type="file" style="position: relative;opacity:0;width:70px;height:40px;margin-right:10px;"  @change="upload" id="fileInput">
            <el-button v-show="CommodityPictures.length !== 3" type="button" class="el-button el-button--primary el-button--small" style="margin-left: -83px">
              <span>本地上传</span>
            </el-button><!-- :disabled="uploadBtn" -->
            <span>图片格式：jpg,jpeg,png,gif,推荐大小800*800.</span>
            <!-- <el-button :disabled="uploadBtn" type="button" class="el-button el-button--primary el-button--small" id="btnClear" @click="clear">清空上传</el-button> -->
            <span style="display: block;font-size: 12px;margin-bottom: 10px"></span>
            <el-col :span="8" v-for="item in CommodityPictures">
              <img style="position: relative;left: 220px;top: -177px;" @click="deldetImg(item)" src="../../assets/delet.png">
              <img :src="item" style="width:200px;height: 200px;border: 1px solid #f0f0f0;">
            </el-col>
            <!-- /*<img style="position: relative;left: 220px;top:-170px;" @click="deldetImg(item)" src="../../assets/delet.png">*/ -->
            <!-- /*<img style="width:150px;margin-top: 20px;margin-right: 20px" v-for="item in CommodityPictures" :src="item">*/ -->
            </el-col>
          </el-col>
          <el-col :span="24">商品详情描述</el-col>
        <el-col style="margin-top: 40px;">
          <div id = 'editor-trigger' style="height: 500px"></div>
          <el-form-item>
            <el-button style="margin-top: 40px;margin-left: 35%" type="primary" @click="Preservation">发布</el-button>
          </el-form-item>
        </el-col>
      <!--添加商品-->
        <el-dialog title="选择补货商品" v-model="shopVisible" :close-on-click-modal="false" :show-close='false'>
          <el-select v-model="value" placeholder="请选择">
              <el-option v-for="item in options" :key="item.value" :label="item.label" :value="item.value"></el-option>
          </el-select>
          <div slot="footer" class="dialog-footer" style="text-align: center;">
            <el-button type="primary" @click.native="addShopInfo">确定</el-button>
            <el-button type="primary" @click.native="shopVisible = false">关闭</el-button>
          </div>
        </el-dialog>
  </el-form>
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
        tableName:'总数量',
        shopAggregate:[],
        shopTable:[],
        value:'',
        options:[],
        _html: '',
        showHtml:'',
        radio:'2',
        filters: {
          name: '',
          price:'',
          value:''
        },
        page:1,
        shopVisible:false,
        listLoading:false,
        table:[],
        CommodityPictures:[],
        value: '',
        data:[],
        formData: new FormData(),
        fileImg: '',
      }
    },
    computed: {
        // 实时更新上传图片的名字，仅读取，值只须为函数
        imageChange: function () {
            return this.fileImg
        }
    },
    methods: {
        radiochange(){
            if(this.radio === 2){
                this.tableName = '总数量'
            }else if(this.radio === 3){
                this.tableName = '总价格'
            }
        },
        // 添加补货商品
        shopAdd(){
          this.shopVisible = true
          this.options = []
          this.getShoplist()
        },
        addShopInfo(){
          this.shopVisible = false
          this.shopAggregate[this.value].id = this.shopAggregate[this.value].goodsSpecs[0].id
          this.shopTable.push(this.shopAggregate[this.value])
        },
        // /删除图片
        deldetImg(src){
          this.CommodityPictures.splice(jQuery.inArray(src, this.CommodityPictures),1)
        },
        getShoplist(){
          const _this = this
          _this.selectSubjectStatus = []
          const params = {
            pageNum:this.page,
            size:1000,
            name:this.name,
            saleStatus:1,
            storeId:state.storeId
          }
          $.ajax({
                type:'POST',
                dataType:'json',
                url:baseUrl+"/api/goods/selectListOfSeller",
                data:JSON.stringify(params),
                contentType:'application/json;charset=utf-8',
                success:function(data){
                  const info = data.data.list
                  _this.shopAggregate = info
                  for (var i = 0; i < info.length; i++) {
                    const obj = {}
                    obj.value = i
                    obj.label = info[i].name
                    _this.options.push(obj)
                  }
                }
            });
      },
        getlist(){
          const _this = this
          _this.options = []
          _this.table = []
          const params = {
            storeId:state.storeId,
            introType:1
          }
          $.ajax({
                type:'POST',
                dataType:'json',
                url:baseUrl+"/api/shopRole/selectOne",
                data:JSON.stringify(params),
                contentType:'application/json;charset=utf-8',
                error: function (XMLHttpRequest, textStatus, errorThrown) {},
                success:function(data){
                  const info = data.data
                  _this.table = info.shopRoles
                  if(info.pictureUrl !== null){
                      _this.CommodityPictures = info.pictureUrl.split(',')
                  }
                  for(var i = 0;i<info.shopRoles.length;i++){
                      const obj = {}
                      obj.value = info.shopRoles[i].id
                      obj.label = info.shopRoles[i].name
                      _this.options.push(obj)
                  }
                }
            });
      },
      click(row){
        for(var i = 0;i<this.table.length;i++){
            if(row === this.table[i].id){
              this.filters.price = this.table[i].addPrice
            }
        }
      },
      PreservationShop(){

      },
      //发布
      Preservation(){
        const _this = this
        const params = {
            // introId:''
            storeId:state.storeId,
            videoUrl:'',
            pictureUrl:this.CommodityPictures.toString(),
            introData:this._html,
            introType:parseInt(this.radio),
            shopRoles:[],
            mixedGoodsList:[]
          }
          for(var i = 0;i<this.table.length;i++){
            const obj = {}
            obj.id = this.table[i].id
            obj.addPrice = this.table[i].addPrice
            params.shopRoles.push(obj)
          }
          for(var i = 0;i<this.shopTable.length;i++){
            const obj = {}
            obj.specId = this.shopTable[i].id
            obj.mixedName = this.shopTable[i].name
            obj.mixedPrice = this.shopTable[i].price
            params.mixedGoodsList.push(obj)
          }
          // params.pictureUrl = this.CommodityPictures.toString()
         $.ajax({
            type:'POST',
            dataType:'json',
            url:baseUrl+"/api/ruleIntroduce/insertOrUpdate ",
            // url:"http://192.168.0.107:8080/api/shopRole/updateAddPrice",
            data:JSON.stringify(params),
            contentType:'application/json;charset=utf-8',
            error: function (XMLHttpRequest, textStatus, errorThrown) {},
            success:function(data){
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




      // 查询补货
      getBuos(){
          const _this = this
          const params = {
            storeId:state.storeId
          }
          $.ajax({
            type:'POST',
            dataType:'json',
            url:baseUrl+"/api/ruleIntroduce/selectOne",
            data:JSON.stringify(params),
            contentType:'application/json;charset=utf-8',
            success:function(data){
              if(data.data.mixedGoodsList !== null){
                 for (var i = 0; i < data.data.mixedGoodsList.length; i++) {
                        const obj = {}
                        obj.name = data.data.mixedGoodsList[i].mixedName
                        obj.price = data.data.mixedGoodsList[i].mixedPrice
                        obj.id = data.data.mixedGoodsList[i].specId
                        _this.shopTable.push(obj)
                  }
                  _this.radio = data.data.introduce.introType
                  _this.CommodityPictures = data.data.introduce.pictureUrl.split(',')
                  _this.showHtml = data.data.introduce.introData
              }
                  _this.initEditor()
             
            }
        });   
      },





        //        清空上传
        clear(){
            let btn = document.getElementById("btnClear");
            let files = document.getElementById("fileInput");
            this.fileImg = '';
            // for IE, Opera, Safari, Chrome
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
            // const flag = this.flag
            if (file) {
                this.fileImg = file.name
                this.formData.append('file', file);
                this.submitUpload()
            } else {
                this.fileImg = ''
                this.formData = new FormData()
            }
        },
        submitUpload(){
                const _this= this;
                _this.$http.post(baseUrl+'/api/attachment/upload', _this.formData, {
                    progress(event) {
                    }
                })
                    .then(response => {
                        const info = JSON.parse(response.bodyText);
          _this.url = info.data[0].baseUri+info.data[0].uri;
          _this.CommodityPictures.push(_this.url)
                    }, error => _this.$emit('complete', 500, error.message))
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
                }
                editor.create()
                _this._html = _this.showHtml

                // _this._html = editor.$txt.html()
                editor.$txt.append(_this.showHtml)
                // _this.CommodityPictures = info.pictureUrl
            }
    },
    mounted() {
       this.getlist()
       this.getBuos()
    }
  }

</script>

<style>
  .noneborder input{
    border: none
  }
  .el-form-item__label{
    text-align: left;
  }
</style>
<template>
  <section>
  	<el-col :span="24" style="padding-bottom: 20px;">基本信息</el-col>
    <el-table :data="table" highlight-current-row v-loading="listLoading" style="width: 502px;">
      <el-table-column type="index" label="序号" width="100">
      </el-table-column>
      <el-table-column prop="name" label="商品名称" width="200">
      </el-table-column>
      <el-table-column prop="addPrice" label="补货价格"  width="200">
        <template scope="scope">
            <el-input type="text" class="noneborder" v-model="scope.row.addPrice"></el-input>
        </template>
      </el-table-column>
    </el-table>
    <el-form :model="filters" label-width="180px" style="margin-left: 40px;margin-top: 40px">
      <el-col :span="24" style="padding-bottom: 20px;">商品图片</el-col>
      <el-col :span="12">
              <el-col :span="24" >
              <input type="file" style="position: relative;opacity:0;width:70px;height:40px;margin-right:10px;"  @change="upload" id="fileInput">
              <el-button :disabled="uploadBtn" type="button" class="el-button el-button--primary el-button--small" style="margin-left: -83px">
                <span>点击上传</span>
              </el-button><!-- :disabled="uploadBtn" -->
              <el-button :disabled="uploadBtn" type="button" class="el-button el-button--primary el-button--small" id="btnClear" @click="clear">清空上传</el-button>
              <span style="display: block;font-size: 12px;margin-bottom: 10px"></span>
              <img style="width:150px;margin-top: 20px;margin-right: 20px" v-for="item in CommodityPictures" :src="item">
              </el-col>
            </el-col>
        <el-col style="margin-top: 40px;">
          <el-form-item label="商品描述">
            <el-input v-model="filters.name" type="textarea" style="width:400px"></el-input>
          </el-form-item>
          <el-form-item>
            <el-button type="primary" @click="Preservation">保存</el-button>
            <el-button>取消</el-button>
          </el-form-item>
        </el-col>
      
  </el-form>
  </section>
</template>

<script>
  import { state } from '../../vuex/state'
  import util from '../../common/js/util'
  import {baseUrl , editUser, addUser } from '../../api/api';

  export default {
    data() {
      return {
        uploadBtn:false,
        filters: {
          name: '',
          price:'',
          value:''
        },
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
        getlist(){
        const _this = this
        _this.options = []
        _this.table = []
        const params = {
          storeId:state.storeId,
          introType:1
        }
        // console.log(params)
        $.ajax({
              type:'POST',
              dataType:'json',
              url:baseUrl+"/api/shopRole/selectOne",
              data:JSON.stringify(params),
              contentType:'application/json;charset=utf-8',
              error: function (XMLHttpRequest, textStatus, errorThrown) {},
              success:function(data){
                const info = data.data
                console.log(info)
                _this.table = info.shopRoles
                _this.CommodityPictures = info.pictureUrl.split(',')
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
        console.log(row)
        for(var i = 0;i<this.table.length;i++){
            if(row === this.table[i].id){
              this.filters.price = this.table[i].addPrice
            }
        }
      },
      Preservation(){
        const _this = this
        const params = {
            // introId:''
            storeId:state.storeId,
            videoUrl:1,
            pictureUrl:'',
            introData:'',
            shopRoles:[]
          }
          for(var i = 0;i<this.table.length;i++){
            const obj = {}
            obj.id = this.table[i].id
            obj.addPrice = this.table[i].addPrice
            params.shopRoles.push(obj)
          }
          params.pictureUrl = this.CommodityPictures.toString()
          console.log(this.CommodityPictures.toString())
          console.log(params)
         $.ajax({
            type:'POST',
            dataType:'json',
            url:baseUrl+"/api/shopRole/updateAddPrice",
            // url:"http://192.168.0.107:8080/api/shopRole/updateAddPrice",
            data:JSON.stringify(params),
            contentType:'application/json;charset=utf-8',
            error: function (XMLHttpRequest, textStatus, errorThrown) {},
            success:function(data){
              console.log(data)
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
            // console.log(file)
            const self = this
            // const flag = this.flag
            if (file) {
                console.log('存在file', file)
                this.fileImg = file.name
                // console.log(this.formData)
                this.formData.append('file', file);
                console.log(this.formData);
                this.submitUpload()
            } else {
                this.fileImg = ''
                console.log('不存在file', file)
                this.formData = new FormData()
            }
        },
        submitUpload(){
            // this.$confirm('确认修改吗？', '提示', {}).then(() => {
                const _this= this;
                _this.$http.post(baseUrl+'/api/attachment/upload', _this.formData, {
                    progress(event) {
                    }
                })
                    .then(response => {
                        const info = JSON.parse(response.bodyText);
                        console.log(info)
                        // const info = response.body
          _this.url = info.data[0].baseUri+info.data[0].uri;
          _this.CommodityPictures.push(_this.url)
          if(_this.CommodityPictures.length === 3){
             this.uploadBtn = true
          }
                    }, error => _this.$emit('complete', 500, error.message))
            // });
        }
    },
    mounted() {
       this.getlist();
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
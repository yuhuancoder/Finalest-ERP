<template>
    <div class="customerClassDetail">
        <el-row class="fixed">
            <el-col :span="24">
                <button @click="isBack" class="erp_bt bt_back">
                    <div class="btImg">
                        <img src="../../../static/image/common/bt_back.png">
                    </div>
                    <span class="btDetail">返回</span>
                </button>     

                <button plain @click="save"  class="erp_bt bt_save">
                    <div class="btImg">
                        <img src="../../../static/image/common/bt_save.png">
                    </div>
                    <span class="btDetail">保存</span>
                </button>
                <button @click="isBack"class="erp_bt bt_cancel">
                    <div class="btImg">
                        <img src="../../../static/image/common/bt_cancel.png">
                    </div>
                    <span class="btDetail">取消</span>
                </button>
                <button  plain @click='saveAdd' class="erp_bt bt_saveAdd">
                    <div class="btImg">
                        <img src="../../../static/image/common/bt_saveAdd.png">
                    </div>
                    <span class="btDetail">保存并新增</span>
                </button>
               <button class="erp_fb_bt bt_add" >
                <div class="btImg">
                    <img src="../../../static/image/common/bt_add.png">
                </div>
                <span class="btDetail">新增</span>
               </button>

              <button class="erp_fb_bt bt_del">
                <div class="btImg">
                    <img src="../../../static/image/common/bt_del.png">
                </div>
                <span class="btDetail">删除</span>
              </button>
            </el-col>
        </el-row>

        <el-row>
            <el-col :span="24" class="pt15">
               <div class="marginAuto">
                    <div class="bgcolor longWidth">
                        <label>上级客户分类</label>
                        <el-select class="classParentId"
                                   clearable filterable 
                                   :class="{redBorder : validation.hasError('addData.classParentId')}" 
                                   placeholder=""
                                   @change="isUpdate"
                                   v-model="addData.classParentId">
                            <el-input placeholder="搜索..."
                                      class="selectSearch"
                                      v-model="parentSearch">
                            </el-input>
                            <el-tree oncontextmenu="return false" ondragstart="return false" onselectstart="return false" onselect="document.selection.empty()" oncopy="document.selection.empty()" onbeforecopy="return false" style="-moz-user-select: none" 
                                     :data="selectParentTree"
                                     v-loading="treeLoading" 
                                     :highlight-current="true"
                                     :render-content="renderContent_moduleParentId"
                                     :props="selectParentProps"
                                     node-key="id"
                                     ref="tree"
                                     :filter-node-method="filterNode"
                                     :default-expanded-keys="expand.expandId_addDataOu"
                                     :expand-on-click-node="false"
                                     @node-click="nodeClick"
                                     >
                            </el-tree>
                             
                            <el-option  v-show="false" v-for="item in selectData.customerClass" :key="item.id" :label="item.className" :value="item.id" :date="item.id"></el-option>
                        
                        
                        
                        </el-select>                    
                    </div>
                    <div class="error_tips">{{ validation.firstError('addData.classParentId') }}</div>
                </div>
            </el-col>

            <el-col :span="24">
                <div class="marginAuto">
                    <div class="bgcolor longWidth">
                        <label><small>*</small>客户分类编码</label>
                        <el-input class="classCode" 
                                  @change="isUpdate"
                                  :class="{redBorder : validation.hasError('addData.classCode')}" 
                                  v-model="addData.classCode">
                        </el-input>
                    </div>
                    <div class="error_tips">{{ validation.firstError('addData.classCode') }}</div>
                </div>    
            </el-col>

            <el-col :span="24">
                <div class="marginAuto">
                    <div class="bgcolor longWidth">
                        <label><small>*</small>客户分类名称</label>
                        <el-input  class="className"
                                    
                                    @change="isUpdate"
                                   :class="{redBorder : validation.hasError('addData.className')}" 
                                   v-model="addData.className"></el-input>
                    </div>
                    <div class="error_tips">{{ validation.firstError('addData.className') }}</div>
                </div>    
            </el-col>
            
            <el-col :span="24">
                <div class="marginAuto">
                    <div class="bgcolor longWidth">
                        <label>备注</label>
                        <el-input class="remark" 
                                  :class="{redBorder : validation.hasError('addData.remark')}" 
                                   @change="isUpdate"
                                  v-model="addData.remark"
                                  type="textarea"
                                  :autosize="{ minRows: 4, maxRows: 4}">
                        </el-input>
                    </div>
                    <div class="error_tips">{{ validation.firstError('addData.remark') }}</div>
                </div>       
            </el-col>
            
            <el-col :span="24">
                <div class="marginAuto">
                    <div class="bgcolor longWidth">
                        <label><small>*</small>状态</label>
                        <el-select  class="status" 
                                    clearable filterable
                                    @change="isUpdate"
                                    :class="{redBorder : validation.hasError('addData.status')}" 
                                    placeholder=""
                                    v-model="addData.status">
                            <el-option v-for="item in status" 
                                       :key="item.itemValue" 
                                       :label="item.itemName" 
                                       :value="item.itemValue">
                            </el-option>
                            
                        </el-select>
                    </div>
                    <div class="error_tips">{{ validation.firstError('addData.status') }}</div>
                </div>    
            </el-col>
      </el-row>
      <el-row>
    <el-col :span="24" class="getPadding">
        <h4 class="h4">审计信息</h4>
        <div>
            <div class="bgcolor"><label>创建人</label><el-input v-model="auditInfo.createdBy" disabled></el-input></div>
            <div class="bgcolor">
                <label>创建时间</label>
               <el-date-picker
                          v-model="auditInfo.createdTime"
                          format="yyyy-MM-dd HH:mm:ss"
                          value-format="yyyy-MM-dd HH:mm:ss"
                          type="date"
                          disabled
                          placeholder="">
                </el-date-picker>
            </div>
            <div class="bgcolor"><label>修改人</label><el-input  v-model="auditInfo.modifiedBy" disabled></el-input></div>
            <div class="bgcolor">
                <label>修改时间</label>
                    <el-date-picker
                          v-model="auditInfo.modifiedTime"
                          format="yyyy-MM-dd HH:mm:ss"
                          value-format="yyyy-MM-dd HH:mm:ss"
                          type="date"
                          disabled
                          placeholder="">
                    </el-date-picker>
            </div>
        </div>                                  
    </el-col>
</el-row>       
       <!-- dialog数据变动提示 -->
        <el-dialog :visible.sync="dialogUserConfirm" class="dialog_confirm_message" width="25%">
            <template slot="title">
                <span class="dialog_font">提示</span>
            </template>
            <el-col :span="24" style="position: relative;">
                <el-col :span="24">
                    <p class="dialog_body_icon"><i class="el-icon-warning"></i></p>
                    <p class="dialog_font dialog_body_message">此操作将忽略您的更改，是否继续？</p>
                </el-col>
            </el-col>
            
            <span slot="footer">
                <button class="dialog_footer_bt dialog_font" @click="sureDoing">确 认</button>
                <button class="dialog_footer_bt dialog_font" @click="dialogUserConfirm = false">取 消</button>
            </span>
        </el-dialog>
        <!-- dialog创建保存失败错误提示-->
        <el-dialog :visible.sync="errorMessage" class="dialog_confirm_message" width="25%">
            <template slot="title">
                <span class="dialog_font">提示</span>
            </template>
            <el-col :span="24" class="detail_message_btnWapper">
                <span @click="detail_message_ifShow = !detail_message_ifShow" class="upBt">详情<i class="el-icon-arrow-down" @click="detail_message_ifShow = !detail_message_ifShow" :class="{rotate : !detail_message_ifShow}"></i></span>
            </el-col>
            <el-col :span="24" style="position: relative;">
                <el-col :span="24">
                    <p class="dialog_body_icon"><i class="el-icon-warning"></i></p>
                    <p class="dialog_font dialog_body_message">{{response.message}}</p>
                </el-col>
                <el-collapse-transition>
                    
                        <el-col :span="24" v-show="detail_message_ifShow" class="dialog_body_detail_message">
                            <vue-scroll :ops="$store.state.option">
                                <span class="dialog_font">{{response.message}}</span>
                                <h4 class="dialog_font dialog_font_bold">其他信息:</h4>
                                <span class="dialog_font">{{response.details}}<br><span :key="index" v-for="(value,index) in response.validationErrors"><span :key="ind" v-for="(val,ind) in value.members">{{val}}</span><br></span></span>
                            </vue-scroll>  
                        </el-col>
                    
                </el-collapse-transition>   
            </el-col>
            
            <span slot="footer">
                <button class="dialog_footer_bt dialog_font dialog_footer_bt_long" @click="errorMessage = false">确 认</button>
            </span>
        </el-dialog>
        <!-- dialog -->
  </div>
  </template>

<script>
    export default({
        data(){
            return{
                //---上级客户树--------
                selectParentTree:[],//选择上级客户
                parentSearch:'',//搜索上级客户
                selectParentProps:{
                    children: 'childNodes',
                    label: 'className',
                    id:'id'
                },
                parentItem:{
                    id:'',
                    className:'',
                },
                expandId:[],//默认展开kehu树节点
                status: [],//状态
                //------------------ 新增客户-------------
                addData:{
                    "groupId": 0,
                    "contactOwner": 1,            
                    "classParentId": '',
                    "classCode": "",
                    "className": "",
                    "classFullname": "1",
                    "classFullPathId": "",
                    "classFullPathName": "",
                    "seq": 0,
                    "status": 1,
                    "remark": "",
                    "mnemonic": "1",
                    
                    },
                    selectData:{//select数据
                        Status001:[],//启用状态
                        customerClass:[],//客户分类
                    },
                    auditInfo:{
                        createdTime:this.GetDateTime(),//创建时间
                        createdBy:this.$store.state.name,//创建人
                        modifiedTime:this.GetDateTime(),//修改人
                        modifiedBy:this.$store.state.name//修改时间
                    },
                choseDoing:'',//存储点击按钮判断信息
                dialogUserConfirm:false,//信息更改提示控制
                update:false,
                firstModify:false,
                treeLoading: false,
                errorMessage:false,//错误信息提示
                detail_message_ifShow:false,
                isAddNew:'',//判断点击的是保存还是保存新增
                response:{
                    details:'',
                    message:'',
                },
                expand:{
                    expandId_addDataOu:[],//默认下拉树形展开id
                    isHere_addDataOu:false,//是否存在id于树形
                    expandId_dialogOu:[],//默认dialog组织树形展开id
                    expandId_mmenu:[],//默认分配功能树形展开id
        }

            }
        },
     validators: {
      'addData.classParentId': function (value) {//上级客户分类，父id
         return this.Validator.value(value).integer();
      },
      'addData.classCode': function (value) {//客户分类编码
         return this.Validator.value(value).required().maxLength(20);
      },
      'addData.className': function (value) {//客户分类名称
         return this.Validator.value(value).required().maxLength(20);
      },
      'addData.remark': function (value) {//备注
          return this.Validator.value(value).maxLength(200);
      },
      'addData.status': function (value) {//状态
         return this.Validator.value(value).required().integer();
      },
    },
    // 创建-------
    created:function(){
            let self = this;
            self.loadParentTree();
            self.loadStatus();
            self.getDefault();
            self.getSelectData();
        },
    watch: {
        parentSearch(val) {
           this.$refs.tree.filter(val);
        },
        // addData:{
        //     handler:function(val,oldVal){
        //         let _this=this;
        //         if(!_this.firstModify){
        //             _this.firstModify=!_this.firstModify;
        //         }else{
        //             _this.update=true
        //         }
        //     },
        //     deep:true,
        // },
    },
    methods: {
        getSelectData(){
            let _this=this;
            // _this.$axios.gets('/api/services/app/DataDictionary/GetDictItem',{dictName:'Status001'}).then(function(res){ 
            // // 启用状态
            // _this.selectData.Status001=res.result;
            // })
           _this.$axios.gets('api/services/app/ContactClassManagement/GetAll',{SkipCount:0,MaxResultCount:100}).then(function(res){ 
            // 客户分类
            _this.selectData.customerClass=res.result.items;
           
            })
        },
           loadCheckSelect(selectName,key){
            let _this=this;
            _this.$nextTick(function () { 
                $('.'+selectName+' .el-tree-node__label').each(function(){
                     if($(this).attr('data-id')==key){
                        $(this).click()
                    }
                })
            })
        },
          defauleExpandTree(model,expandName,response,responseKey,children){
               let _this=this;
            // console.log(model!='');
            if(model!=''){//model为树形下拉默认值，即渲染数据。如果存在，递归tree
                $.each(response,function(index,val){
                    if(val[responseKey]!==_this.addData[model]){
                        _this.expand[expandName]=[_this.addData[model]]
                    }else{
                        $.each(val[children],function(index1,val1){
                            if(val1[responseKey]==_this.addData[model]){
                                _this.expand[expandName]=[_this.addData[model]]
                            }else{
                                _this.defauleExpandTree(model,expandName,val1[children],responseKey,children)
                            }
                        })
                    }
                })
            }else{
                 $.each(response,function(index,value){
                    if(index==0){
                        if(typeof(value)!='undefined'&&value!=null&&value[responseKey]!=null&&value[responseKey]!=''&&typeof(value[responseKey])!='undefined'){
                            _this.expand[expandName]=[value[responseKey]]
                        }
                        
                    }
                })
   
            }
        },
        //---加载数据上级商品树-------------------------------------------  
        loadParentTree(){
            let self=this;
            self.treeLoading=true;
            self.$axios.gets('api/services/app/ContactClassManagement/GetTreeList',{Ower:1}).then(function(res){
                console.log(res)
                self.selectParentTree=res;
                self.treeLoading = false;
                // self.expandId=self.defauleExpandTree(res,'id')
                // console.log(self.expandId);
                  self.defauleExpandTree('classParentId','expandId_addDataOu',res,'id','children')
                    if(self.expand.expandId_addDataOu<1){
                        self.expand.expandId_addDataOu=[self.selectParentTree[0].id]
                    
                    }
                self.loadCheckSelect('classParentId',self.addData.classParentId)
            },function(res){
                self.treeLoading=false;
                
            })
        },
        getDefault(){
            let self=this;
            if(self.$route.params.id!="default"){
                self.addData.classParentId=parseInt(self.$route.params.id);
                self.parentItem.className = '111111';
                self.parentItem.id=self.$route.params.id;
            }
        },
        //加载状态下拉框
        loadStatus:function(){
            let self = this;
            self.$axios.gets('/api/services/app/DataDictionary/GetDictItem',{dictName:'Status001'}).then(function(res){
                // console.log(res)        
            self.status = res.result;            
            },function(res){
                
            })
        },
            GetDateTime: function () {
            var date = new Date();
            var seperator1 = "-";
            var seperator2 = ":";
            var month = date.getMonth() + 1;
            var strDate = date.getDate();
            if (month >= 1 && month <= 9) {
                month = "0" + month;
            }
            if (strDate >= 0 && strDate <= 9) {
                strDate = "0" + strDate;
            }
            var currentdate = date.getFullYear() + seperator1 + month + seperator1 + strDate
                + " " + date.getHours() + seperator2 + date.getMinutes()
                + seperator2 + date.getSeconds();
            return currentdate;
        },
    //-----------------------------------------------------
        
        //---保存---------------------------------------------
           save(){
            let self=this; 
                self.$validate().then(function (success) {
                if (success) {
                   self.$axios.posts('/api/services/app/ContactClassManagement/Create',self.addData).then(function(res){  
                        // console.log(res.result);
                        self.addData.id=res.result.id;
                        self.$store.state.url='/customerClass/customerClassModify/'+res.result.id
                        self.$router.push({path:self.$store.state.url})
                        self.open('保存成功','el-icon-circle-check','successERP');
                        self.dialogUserConfirm=false;
                    },function(res){
                        // self.open('保存失败','el-icon-error','faildERP');
                         if(res && res!=''){ 
                            self.getErrorMessage(res.error.message,res.error.details,res.error.validationErrors)}
                            self.dialogUserConfirm=false;
                            self.errorMessage=true;
                    })
                }
            });
        },
        saveAdd:function(){
            let self = this;
            self.$validate().then(function (success) {
                // console.log(success);
                if (success) {
                    self.$axios.posts('/api/services/app/ContactClassManagement/Create',self.addData).then(function(res){
                        //  self.addData=res.result;
                         self.addData.id=res.result.id;
                         self.$store.state.url='/customerClass/customerClassDetail/default'
                         self.$router.push({path:self.$store.state.url})
                         self.open('保存成功','el-icon-circle-check','successERP');
                          self.clearData();
                          self.validation.reset();
                          self.update=false;
                        //   self.firstModify=true;
                         console.log(self.firstModify)
                    },function(res){    
                        // self.open('保存失败','el-icon-error','faildERP');
                        if(res && res!=''){ 
                        self.getErrorMessage(res.error.message,res.error.details,res.error.validationErrors)}
                        self.dialogUserConfirm=false;
                        self.errorMessage=true;
                    })
                }
            });
        },
        //----------------------------------------/api/services/app/User/Create---------------
        //---open---路由切换--------------------------------------
        open(tittle,iconClass,className) {
            this.$notify({
            position: 'bottom-right',
            iconClass:iconClass,
            title: tittle,
            showClose: false,
            duration: 3000,
            customClass:className
            });
        },
        // ----------------------------按钮组------------------------
        back(row){

            this.$store.state.url='/customerClass/customerClassList/default'
            this.$router.push({path:this.$store.state.url})//点击切换路由
        },
        sureDoing(){
            let self=this;
            if(self.choseDoing=='back'){
                self.back()
                self.dialogUserConfirm=false;
            }else if(self.choseDoing=='Cancel'){
                self.Cancel();
                self.dialogUserConfirm=false;
            }
            },
        Cancel(){
            let self=this;
            self.clearData();
            // self.update=false;
        },
        goModify:function(id){
            // console.log(id)
            this.$store.state.url='/customerClass/customerClassModify/'+id
            this.$router.push({path:this.$store.state.url})//点击切换路由
        },
        goDetail(){//点击新增跳转
            this.$store.state.url='/customerClass/customerClassDetail/default'
            this.$router.push({path:this.$store.state.url})//点击切换路由
        },
        // 清除数据-------------------------
         clearData(){
          let self=this;
            self.addData={
                "groupId": 0,
                "contactOwner": 1,            
                "classParentId": '',
                "classCode": "",
                "className": "",
                "classFullname": "1",
                "classFullPathId": "",
                "classFullPathName": "",
                "seq": 0,
                "status": 1,
                "remark": "",
                "mnemonic": "1",
                // "createdBy" :'',
                // "createdTime"  :''
            }
            // self.getDefault()
            self.validation.reset(); 
        },
        //---------------------------------------------------------
        //---下拉树------------------------------------------------.
        filterNode(value, data) {
            // console.log(data)
            if (!value) return true;
                return data.className.indexOf(value) !== -1;
        },

        nodeClick:function(data,node,self){
            let _this = this;
            _this.parentItem.id = data.id;
            _this.parentItem.className = data.className;
            $(self.$el).parents('.el-select-dropdown__list').children('.el-select-dropdown__item').each(function(index){
                if($(this).attr('date')==data.id){
                    $(this).click()
                }
            })
             $(self.$el).parents('.el-select-dropdown__list').children('.el-select-dropdown__item').css({top:$(self.$el).offset().top-$(self.$el).parents('.el-select-dropdown__list').offset().top+26,})
        },
        //-------------按钮操作-----------
        isBack(){
            let self=this;
            if(self.update){
                self.dialogUserConfirm=true;
                self.choseDoing='back'
            }else{
                self.back()
            }
        },
        isUpdate(){//判断是否修改过信息
            this.update=true;
        },
        isCancel(){
            let self=this;
            if(self.update){
                self.dialogUserConfirm=true;
                self.choseDoing='Cancel'
            }
        },

        //---错误提示-------------------------------------------
        showErrprTips(e){
            $('.tipsWrapper').each(function(){
                if($(e.target).parent('.el-input').hasClass($(this).attr('name'))){
                    $(this).addClass('display_block')
                }else{
                    $(this).removeClass('display_block')
                }
            })
        },
        showErrprTipsSelect(e){
            $('.tipsWrapper').each(function(){
                if($(e.target).parent('.el-input').parent('.el-select').hasClass($(this).attr('name'))){
                    $(this).addClass('display_block')
                }else{
                    $(this).removeClass('display_block')
                }
            })
        },
        showErrprTipsRangedate(e){
            $('.tipsWrapper').each(function(){
                if($(e.$el).hasClass($(this).attr('name'))){
                    $(this).addClass('display_block')
                }else{
                    $(this).removeClass('display_block')
                }
            })
        },
         getErrorMessage(message,details,validationErrors){
            let _this=this;
            _this.response.message='';
            _this.response.details='';
            _this.response.validationErrors=[];
            if(details!=null && details){
                _this.response.details=details;
            }
            if(message!=null && message){
                _this.response.message=message;
            }
            if(message!=null && message){
                _this.response.validationErrors=validationErrors;
            }
        },
         renderContent_moduleParentId(h, { node, data, store }){
            if(typeof(data.childNodes)!='undefined' && data.childNodes!=null && data.childNodes.length>0){
                return (
                    <span class="el-tree-node__label" data-id={data.id}>
                    <i aria-hidden="true" class="preNode fa fa-folder-open" style="color:#f1c40f;margin-right:5px"></i>
                        {data.className}
                    </span>
                );
            }else{
                 return (
                    <span class="el-tree-node__label" data-id={data.id}>
                    <i class="preNode fa fa-file" aria-hidden="true" style="color:#f1c40f;margin-right:5px"></i>
                        {data.className}
                    </span>
                );
            }
        },
    }

})
</script>



<style scoped>
.pt15{
    padding-top: 15px;
}

.customerClassDetail  .errorTips{
    margin-bottom: 10px;
    margin-top: -10px;
}
.customerClassDetail .getPadding {
    padding: 0 10px;
}
 .customerClassDetail .el-row{
   padding: 15px 0;
    border-bottom: 1px solid #e4e4e4;
    background-color: #fff;
 }

 .customerClassDetail .el-row:first-child{
   padding: 7px 0;
   border-bottom: 1px solid #e4e4e4;
  }
 .customerClassDetail .el-row:last-child {
  padding-bottom: 15px;

 }
/* .selectSearch{
  position: fixed;
  width: 328px;
  z-index: 10002;
} */
.el-tree-node__children{
  background-color: #fff;
}
  .customerClassDetail .bgcolor .isGive{
    display: block;
    float: left;
    height: 100%;
    line-height: 35px;
  }
.customerClassDetail .bgcolor.longWidth{
    margin-right: 0;
    width: 421px;
    height:auto;
    float: left;
  }
  .marginAuto{
      margin: auto;
      width: 550px;
  }
  .error_tips{
      color: red;
      font-size: 12px;
      float: left;
      height: 35px;
      line-height: 35px;
  }
  .customerClassDetail .bgcolor.longWidth .el-input,
  .customerClassDetail .bgcolor.longWidth .el-textarea,
  .customerClassDetail .bgcolor.longWidth .el-select{
      width: calc(100% - 90px)
  }
  .customerClassDetail .bgcolor.longWidth label{
    width:80px;
  }
 .customerClassDetail .bgcolor.longWidth .el-textarea{
   font-size: 12px;
 } 
 .customerClassDetail .bgcolor.longWidth .addZoo{
   float: left;
   width: calc(100% - 82px)
 }
.customerClassDetail .bgcolor.longWidth .add{
    display: block;
    width: 35px;
    height: 35px;
    border-radius: 3px;
    background-color: #c7c7c7;
    color: #fff;
    text-align: center;
    line-height: 35px;
    text-decoration: none;
    font-size: 23px;
    font-weight: bold;
    margin-bottom: 10px;
}
.customerClassDetail .bgcolor.longWidth .addRole{
  text-align: center;
  line-height: 35px;
  display: inline-block;
  width: 66px;
  height: 35px;
  background-color: #f2f2f2;
  border: none;
  border-radius: 3px;
  font-size: 12px;
  margin-right: 10px;
  cursor: pointer;
  position: relative;
}
.customerClassDetail .bgcolor.longWidth .add:hover{
    background-color: #354052;
}
.customerClassDetail .bgcolor.longWidth .addRole i{
  position: absolute;
  right: -4px;
  top: -4px;
  color: #cccccc;
}
.customerClassDetail .bgcolor.longWidth .addRole:hover i{
  color:#f66;
}
</style>


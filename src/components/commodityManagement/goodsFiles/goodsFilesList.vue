<template>
    <div class="roleList">
        <el-row class="bg-white">
            <el-col :span="ifWidth?5:0" v-show="ifWidth" class="search-container">
                <el-row class="h48 pl15">
                    <el-col :span="18" class="btn-for-search">
                        <img src="../../../../static/image/common/search_btn.png">
                        <span>查询</span>
                    </el-col>
                    <el-col :span="2" :offset="4">
                        <span class="fs12 search_info_open" @click="closeLeft">-</span>
                    </el-col>
                </el-row>
                <div class="mt20 bgcolor smallBgcolor"><label>商品编码</label><el-input v-model="searchData.ProductCode" placeholder=""></el-input></div>
                <div class="bgcolor smallBgcolor"><label>商品名称</label><el-input v-model="searchData.ProductName" placeholder=""></el-input></div>
                <div class="bgcolor smallBgcolor">
                    <label>上市时间(起)</label>
                     <el-date-picker
                     v-model="searchData.SaleDateStart"
                    format="yyyy.MM.dd"
                    type="date" 
                    align="center"></el-date-picker>
                </div>
                <div class="bgcolor smallBgcolor">
                    <label>上市时间(终)</label>
                    <el-date-picker
                    v-model="searchData.SaleDateEnd"
                    format="yyyy.MM.dd"
                    type="date" 
                    align="center"></el-date-picker>
                </div>
                <div class="bgcolor smallBgcolor">
                    <label>类目</label>
                    <el-select clearable filterable   v-model="searchData.CategoryId" placeholder="">
                       <el-input
                        placeholder="搜索..."
                        class="selectSearch"
                        v-model="search_categoryId">
                        </el-input>
                        <el-tree
                        :default-expanded-keys="expand_categoryId"
                        :render-content="renderContent_categoryId"
                        :data="selectTree_categoryId"
                        :highlight-current="true"
                        :props="selectProps_categoryId"
                        node-key="id"
                        ref="tree"
                        :filter-node-method="filterNode_categoryId"
                        :expand-on-click-node="false"
                        @node-click="nodeClick_categoryId"
                        >
                        </el-tree>
                        <el-option class="select_tree_option" :key="item_categoryId.id" :label="item_categoryId.categoryName" :value="item_categoryId.id">
                        </el-option>
                    </el-select>
                </div>
                <div class="bgcolor smallBgcolor">
                    <label>品牌</label>
                    <el-select clearable filterable   v-model="searchData.BrandId" placeholder="">
                        <el-option v-for="item in selectData.brand" :key="item.id" :label="item.brandName" :value="item.id">
                        </el-option>
                    </el-select>
                </div>
                <div class="bgcolor smallBgcolor">
                    <label>状态</label>
                    <el-select clearable filterable   v-model="searchData.Status" placeholder="">
                        <el-option v-for="item in selectData.Status001" :key="item.itemValue" :label="item.itemName" :value="item.itemValue">
                        </el-option>
                    </el-select>
                </div>
                <div class="bgcolor smallBgcolor">
                    <label>材质</label>
                    <el-select v-model="searchData.Status" clearable filterable placeholder="">
                       
                    </el-select>
                </div>
                <div class="bgcolor smallBgcolor">
                    <label>款式</label>
                    <el-select v-model="searchData.Status" clearable filterable  placeholder="">
                       
                    </el-select>
                </div>
                <div class="bgcolor smallBgcolor">
                    <label></label>
                    <span class="search-btn" @click="SimpleSearchClick">查询</span>
                </div>
            </el-col>

            <el-col :span="ifWidth?19:24" class="border-left">
                <el-row class="h48 ">
                    <el-col :span="ifWidth?0:2" class="search-block">
                        <div @click="openLeft">
                            <img src="../../../../static/image/common/search_btn.png">
                            <span>查询</span>
                            <span class='open-search'>+</span>
                        </div>
                    </el-col>
                    <el-col :span="ifWidth?24:22" class="pt5">
                        <button @click="goDetail" class="erp_bt bt_add"><div class="btImg"><img src="../../../../static/image/common/bt_add.png"></div><span class="btDetail">新增</span></button>
                        <button @click="confirm" class="erp_bt bt_del"><div class="btImg"><img src="../../../../static/image/common/bt_del.png"></div><span class="btDetail">删除</span></button>
                        <button class="erp_bt bt_in"><div class="btImg"><img src="../../../../static/image/common/bt_inOut.png"></div><span class="btDetail">导入</span></button>
                        <button class="erp_bt bt_out bt_width">
                            <div class="btImg"><img src="../../../../static/image/common/bt_inOut.png"></div>
                            <span class="btDetail">导出</span>
                            <div class="btRightImg"><img src="../../../../static/image/common/bt_down_right.png"></div>
                        </button>
                        <button class="erp_bt bt_start"><div class="btImg"><img src="../../../../static/image/common/bt_start.png"></div><span class="btDetail">启用</span></button>
                        <button class="erp_bt bt_stop"><div class="btImg"><img src="../../../../static/image/common/bt_stop.png"></div><span class="btDetail">停用</span></button>   
                              
                        <div class="search_input_group">
                            <div class="search_input_wapper"  @keyup.enter="submitSearch">
                                <el-input
                                    placeholder="搜索..."
                                    v-model="Name"
                                    class="search_input">
                                    <i slot="prefix" class="el-input__icon el-icon-search"></i>
                                </el-input>
                            </div>
                            <div class="search_button_wrapper" @click="dialogUserDefined = true">
                                <button class="userDefined" >
                                    <i class="fa fa-cogs" aria-hidden="true"></i>自定义
                                </button>
                            </div>
                        </div>
                    </el-col>   
                </el-row>
                <!-- dialog -->
                <el-dialog :visible.sync="dialogUserDefined" class="dialogUserDefined">
                    <template slot="title">
                        <span>自定义<small>(设置显示字段)</small></span>
                    </template>
                     <el-table
                        :data="tableData" 
                        border 
                        style="width: 100%" 
                        stripe 
                        ref="multipleTable">
                            <el-table-column label="序号" fixed="left">
                                 <template slot-scope="scope">
                                    {{scope.$index + 1}}
                                </template>
                            </el-table-column>
                            <el-table-column prop="field" label="字段" fixed="left"></el-table-column>
                            <el-table-column prop="field" label="操作" fixed="left">
                                <template slot-scope="scope">
                                    <el-switch
                                        v-model="tableData[scope.$index].value"
                                        active-color="#13ce66">
                                    </el-switch>
                                </template>
                            </el-table-column>
                        </el-table>   
                        <span slot="footer" class="dialog-footer">
                            <el-button type="primary">确 定</el-button>
                            <el-button>取 消</el-button>
                        </span>
                </el-dialog>
                <!-- dialog -->
                <el-row>

                    <el-col :span='24'>
                        <el-table 
                        v-loading="tableLoading"
                        :data="tableData" 
                        border 
                        style="width: 100%" 
                        stripe 
                        @selection-change="handleSelectionChange" 
                        ref="multipleTable">
                            <el-table-column type="selection" fixed="left"></el-table-column>
                            <el-table-column prop="productCode" label="商品编码" fixed="left">
                                <template slot-scope="scope">
                                    <el-button type="text"  @click="see(scope.row)">{{scope.row.productCode}}</el-button>
                                </template>
                            </el-table-column>
                            <el-table-column prop="productName" label="商品名称" fixed="left">
                                <template slot-scope="scope">
                                    <el-button type="text"  @click="see(scope.row)">{{scope.row.productName}}</el-button>
                                </template>
                            </el-table-column>
                            <el-table-column prop="statusTValue" label="状态">
                                <template slot-scope="scope">
                                    <span v-if="scope.row.statusTValue=='启用'" style="color:#39CA77;">{{scope.row.statusTValue}}</span>
                                    <span v-else-if="scope.row.statusTValue=='停用'" style="color:#FF6666;">{{scope.row.statusTValue}}</span>
                                    <span v-else>{{scope.row.statusTValue}}</span>
                                </template>
                            </el-table-column>
                            <el-table-column prop="barcode" label="商品条码">
                            </el-table-column>
                            <el-table-column prop="brandId_BrandName" label="品牌"></el-table-column>
                            <el-table-column prop="categoryId_CategoryName" label="类目"></el-table-column>
                            <el-table-column label="上市日期" width="160">
                                <template slot-scope="scope">
                                    <el-date-picker
                                    format="yyyy.MM.dd"
                                    v-model="tableData[scope.$index].saleDate" 
                                    type="datetime" 
                                    readonly
                                    align="center"></el-date-picker>
                                </template>
                            </el-table-column>
                            <el-table-column label="操作" fixed="right">
                                 <template slot-scope="scope">
                                     <el-button type="text"  @click="see(scope.row)" >查看</el-button>
                                     <el-button type="text"  @click="confirmDelThis(scope.row)">删除</el-button>
                                    <!-- <el-button type="text"  @click="see(scope.row)" >查看</el-button> -->
                                </template>
                            </el-table-column>
                        </el-table>
                        <el-pagination
                        style="margin-top:20px;" 
                        class="text-right" 
                        background layout="total,prev, pager, next,jumper" 
                        @current-change="handleCurrentChange"
                        :current-page="pageIndex"
                        :page-size="oneItem"
                        :total="totalItem">
                        </el-pagination>   
                    </el-col>
                </el-row>

            </el-col>
        </el-row>
        <!-- dialog是否删除提示 -->
        <el-dialog :visible.sync="dialogUserConfirm" class="dialog_confirm_message" width="25%">
            <template slot="title">
                <span class="dialog_font">提示</span>
            </template>
            <el-col :span="24" style="position: relative;">
                <el-col :span="24">
                    <p class="dialog_body_icon"><i class="el-icon-question"></i></p>
                    <p class="dialog_font dialog_body_message">确认删除？</p>
                </el-col>
            </el-col>
            
            <span slot="footer">
                <button class="dialog_footer_bt dialog_font" @click="sureAjax">确 认</button>
                <button class="dialog_footer_bt dialog_font" @click="dialogUserConfirm = false">取 消</button>
            </span>
        </el-dialog>
        <!-- dialog -->
       <!-- dialog错误信息提示 -->
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
                    <p class="dialog_font dialog_body_message">信息提报有误!</p>
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
    export default{
        data(){
            return {
                 // 错误信息提示开始
                detail_message_ifShow:false,
                errorMessage:false,
                // 错误信息提示结束
//--------------确认删除开始-----------------               
                dialogUserConfirm:false,//用户删除保存提示信息
                row:{},//存储用户点击删除条目数据
                choseAjax:'',//存储点击单个删除还是多天删除按钮判断信息
                multipleSelection: [],//复选框选中数据
//--------------确认删除开始-----------------    
                search_categoryId:'',//类目树搜索
                selectTree_categoryId:[],//类目树数据
                selectProps_categoryId: {//类目树默认属性
                    children: 'childNodes',
                    label: 'categoryName',
                    id:'id'
                },
                expand_categoryId:[],////类目树默认展开节点id
                item_categoryId:{//类目树选中节点
                    id:'',
                    categoryName:''
                },
                tableLoading:false,
                searchData:{
                    ProductCode: "",//编码
                    ProductName: "",//名称
                    SaleDateStart:'',//上市时间(始)
                    SaleDateEnd: '',//上市时间(终)
                    CategoryId: '',//类目
                    BrandId: '',//品牌
                    Status: '',//启用状态
                    // OuType: '',//材质
                    // OuType: '',//款式
                },
                searchDataClick:{},
                tableSearchData:{},
                
                tableData:[],

                pageIndex:1,//分页的当前页码
                totalPage:0,//当前分页总数
                oneItem:10,//每页有多少条信息
                page:1,//当前页
                treeCheck:[],
                isClick:[],
                load:'loadTableData',
                totalItem:0,//总共有多少条消息
                ifWidth:true,
                dialogUserDefined:false,//dialog

                response:{
                details:'',
                message:'',
                validationErrors:[],
                },
                Name:'',//右上角模糊查询
                selectData:{//select数据
                    Status001:[],//启用状态
                    brand:[],//商品品牌
                },
            }
        },
        watch: {
            search_categoryId(val) {
                this.$refs.tree.filter(val);
            },
        },  
        created:function(){       
                let _this=this;
                _this.loadTree_categoryId();
                _this.loadTableData();
                _this.getSelectData()
             },
        methods:{
            closeLeft:function(){
               let self = this;
               self.ifWidth = false;
           },
           openLeft:function(){
               let self = this;
               self.ifWidth = true;
           },
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
            getSelectData(){
                let _this=this;
                _this.$axios.gets('/api/services/app/DataDictionary/GetDictItem',{dictName:'Status001'}).then(function(res){ 
                    // 启用状态
                    _this.selectData.Status001=res.result;
                })
                _this.$axios.gets('/api/services/app/BrandManagement/GetAll',{SkipCount:0,MaxResultCount:1}).then(function(res){ 
                    // 商品品牌
                    
                    if(res.result.totalCount==0){
                        _this.selectData.brand=[]
                    }else{
                        _this.$axios.gets('/api/services/app/BrandManagement/GetAll',{SkipCount:0,MaxResultCount:res.result.totalCount})
                        .then(function(response){
                            _this.selectData.brand=response.result.items;
                        })
                    }
                    
                })
            },
            loadTableData(){//表格
                let _this=this;
                _this.ajaxTable({SkipCount:(_this.page-1)*_this.oneItem,MaxResultCount:_this.oneItem},'loadTableData')
            },
            ajaxTable(data,event){
                 let _this=this;
                _this.tableLoading=true
                _this.$axios.gets('/api/services/app/ProductManagement/GetListByCondition',data).then(function(res){ 
                    _this.load=event;
                    _this.tableData=res.result.items;
                    _this.totalItem=res.result.totalCount
                    _this.totalPage=Math.ceil(res.result.totalCount/_this.oneItem);
                    _this.tableLoading=false;
                    },function(res){
                    _this.tableLoading=false;
                })
            },
            filterNode_categoryId(value, data) {
                if (!value) return true;
                return data.categoryName.indexOf(value) !== -1;
            },
            loadTree_categoryId(){
                let _this=this;
                _this.$axios.gets('/api/services/app/CategoryManagement/GetCategoryTree')
                .then(function(res){
                    _this.selectTree_categoryId=res;
                    _this.expand_categoryId=_this.defauleExpandTree(res,'id')
                },function(res){
                })
            },
            nodeClick_categoryId(data,node,self){
                let _this=this;
                _this.item_categoryId.id=data.id;
                _this.item_categoryId.categoryName=data.categoryName;
                _this.$nextTick(function(){
                    // $(self.$el).parents('.el-select-dropdown__list').children('.el-select-dropdown__item').click();
                    $(self.$el).parents('.el-select-dropdown__list').children('.el-select-dropdown__item').css({top:$(self.$el).offset().top-$(self.$el).parents('.el-select-dropdown__list').offset().top+26,}).click();
                })
                // $(self.$el).parents('.el-select-dropdown__list').children('.el-select-dropdown__item').each(function(index){
                // if($(this).attr('date')==data.id){
                //     $(this).click()
                // }
            // })
                
            },
            handleCurrentChange(val) {//页码改变
                 let _this=this;
                 _this.page=val;
                 if(_this.load=="loadTableData"){
                     _this.ajaxTable({SkipCount:(_this.page-1)*_this.oneItem,MaxResultCount:_this.oneItem},"loadTableData")
                 }else if(_this.load=="SimpleSearch"){
                     _this.SimpleSearch();
                 }else if(_this.load=="submitSearch"){
                       _this.ajaxTable({ProductName:_this.Name,SkipCount:(_this.page-1)*_this.oneItem,MaxResultCount:_this.oneItem},"submitSearch");
                 }
            },
            SimpleSearchClick(){
                let _this=this;
                 
                 _this.searchDataClick={
                    ProductCode:_this.searchData.ProductCode,//
                    ProductName: _this.searchData.ProductName,//
                    SaleDateStart: _this.searchData.SaleDateStart,//
                    SaleDateEnd: _this.searchData.SaleDateEnd,
                    CategoryId:_this.searchData.CategoryId,
                    BrandId:_this.searchData.BrandId,
                    Status:_this.searchData.Status,
                }
                _this.page=1;
                _this.SimpleSearch();
            },
            SimpleSearch(){//简单搜索
                 let _this=this;
                 _this.tableLoading=true;
                 _this.searchDataClick.SkipCount=(_this.page-1)*_this.oneItem;
                 _this.searchDataClick.MaxResultCount=_this.oneItem;
                _this.ajaxTable(_this.searchDataClick,"SimpleSearch")
            },
            goDetail(){
                this.$store.state.url='/goodsFiles/goodsFilesDetail/default'
                this.$router.push({path:this.$store.state.url})//点击切换路由
            },
             handleSelectionChange(val) {//点击复选框选中的数据
                this.multipleSelection = val;
            },
            confirm(){//多项删除
                let _this=this;
                _this.choseAjax='rows'
                if(_this.multipleSelection.length>0){
                _this.dialogUserConfirm=true;
                }
            },
            confirmDelThis(row){//单项删除
                let _this=this;
                _this.choseAjax='row'
                _this.row=row;
                _this.dialogUserConfirm=true;
            },
            sureAjax(){
                let _this=this;
                if(_this.choseAjax=='row'){
                    _this.delThis()
                }else if(_this.choseAjax=='rows'){
                    _this.delRow()
                }
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
            delThis(){//删除行
                let _this=this;
                _this.$axios.deletes('/api/services/app/ProductManagement/Delete',{id:_this.row.id})
                .then(function(res){
                     _this.dialogUserConfirm=false;
                    _this.open('删除成功','el-icon-circle-check','successERP');
                    _this.loadTableData();
                },function(res){
                    if(res && res!=''){ _this.getErrorMessage(res.error.message,res.error.details,res.error.validationErrors)}
                     _this.dialogUserConfirm=false;
                      _this.errorMessage=true;
                })
            },
            delRow(){
                let _this=this;
                let data={
                    "createList": [],
                    "updateList": [],
                    "deleteList": _this.multipleSelection
                }
                _this.$axios.posts('/api/services/app/ProductManagement/CUDAggregate',data)
                .then(function(res){
                     _this.dialogUserConfirm=false;
                    _this.open('删除成功','el-icon-circle-check','successERP');
                        _this.loadTableData();
                },function(res){
                    if(res && res!=''){ _this.getErrorMessage(res.error.message,res.error.details,res.error.validationErrors)}
                    
                    _this.dialogUserConfirm=false;
                    _this.errorMessage=true;
                })
            },
            see(row){
                this.$store.state.url='/goodsFiles/goodsFilesModify/'+row.id
                this.$router.push({path:this.$store.state.url})//点击切换路由
            },
             submitSearch(){
                let _this=this;
                _this.page=1
                _this.ajaxTable({ProductName:_this.Name,SkipCount:(_this.page-1)*_this.oneItem,MaxResultCount:_this.oneItem},"submitSearch");
            },
            defauleExpandTree(data,key){
                if(typeof(data[0])!='undefined'
                && data[0]!=null 
                && typeof(data[0][key])!='undefined'
                && data[0][key]!=null
                && data[0][key]!=''){
                    return [data[0][key]]
                }
            },
            renderContent_categoryId(h, { node, data, store }){
                if(typeof(data.childNodes)!='undefined' && data.childNodes!=null && data.childNodes.length>0){
                    return (
                        <span class="el-tree-node__label" data-id={data.id}>
                        <i aria-hidden="true" class="preNode fa fa-folder-open" style="color:#f1c40f;margin-right:5px"></i>
                            {data.categoryName}
                        </span>
                    );
                }else{
                    return (
                        <span class="el-tree-node__label" data-id={data.id}>
                        <i class="preNode fa fa-file" aria-hidden="true" style="color:#f1c40f;margin-right:5px"></i>
                            {data.categoryName}
                        </span>
                    );
                }
            },
            
        },
    }
</script>

<style scoped>
.bg-white{
    background: white;
    border-radius: 3px;
}
.h48{
    height: 48px;
    line-height: 48px;
    border-bottom: 1px solid #E4E4E4;
}
.mt20{
    margin-top: 10px;
}
.pl15{
    padding-left: 15px;
}
.pt5{
    padding-top: 5px;
}
.fs12{
    font-size: 12px;
}
.border-left{
    border-left: 1px solid #E4E4E4;
    min-height: 438px;
}
.btn{
    display: inline-block;
    width: 100%;
    text-align: center;
    height: 30px;
    line-height: 30px;
    background: rgba(130, 170, 252, 1);
    color: white;
    border-radius: 3px;
    cursor: pointer;
}
.open-search{
    background-image: url(../../../../static/image/common/btn-circle.png);
    background-repeat: no-repeat;
    background-position: center;
    color: #E3E3E3;
    font-size: 12px;
    width: 19px;
    float: right;
    margin-right: 10px;
} 
</style>

<style>
.roleList .el-button+.el-button{
    margin-left: 0;
}

</style>
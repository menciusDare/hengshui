<template>
    <!-- 主体 -->
    <div class="main">
        <div class="main-top">
            <div class="breadcrumb">
                <a href="#"><i class="icon icon-shenbaodengji menu-icon"></i>申报登记</a><span class="sep">&gt;</span><span
                    class="text-primary">产废年报</span>
            </div>
        </div>
        <div class="panel">
            <div class="panel-bd">
                <!-- 收起后样式名.filter-more-mini -->
                <div style="padding: 20px 0px 0 2px;">
                    <ul class="filter-box">
                        <li class="filter-item">
                            <div class="filter-label">所在地：</div>
                            <div class="filter-con">
                                <div class="el-select w160">
                                    <el-cascader
                                            ref="regionElCascader"
                                            :disabled="isDisabled"
                                            @change="searchRegionChange"
                                            v-model="search.regionCode"
                                            :options="regionOptions"
                                            :props="{ checkStrictly: true }"
                                            >
                                    </el-cascader>
                                </div>
                            </div>
                        </li>
                        <li class="filter-item">
                            <div class="filter-label">企业类型：</div>
                            <div class="filter-con">
                                <div class="el-select w124">
                                    <el-select v-model="search.entType" clearable>
                                        <el-option
                                                v-for="item in entTypeOptions"
                                                :key="item.value"
                                                :label="item.label"
                                                :value="item.value">
                                        </el-option>
                                    </el-select>
                                </div>
                            </div>
                        </li>
                        <li class="filter-item">
                            <div class="filter-label">提交年份：</div>
                            <div class="filter-con">
                                <el-date-picker style="width: 150px;"
                                                :clearable="false"
                                                @change = "submitYearChange"
                                                v-model="search.reportYear"
                                                :picker-options="pickerOptions"
                                                type="year" value-format="yyyy"
                                                placeholder="选择年">
                                </el-date-picker>
                            </div>
                        </li>
                        <li class="filter-item">
                            <div class="filter-con">
                                <div class="el-input w180 el-input-group el-input--prefix">
                    <span class="el-input__prefix"><i class="el-input__icon el-icon-search"></i>
                    </span><input type="text" autocomplete="off" placeholder="搜索企业名称" v-model="search.entName"
                                  class="el-input__inner">
                                </div>
                            </div>
                        </li>
                        <li class="filter-item">
                            <div class="filter-btn">
                                <!--<button type="button" @click="searchEntManagementPlanReport" class="button-type">
                                    <span>查询</span></button>-->
                                <el-button type="button" size="medium"  @click="searchEntManagementPlanReport" class="el-button el-button--primary">
                                    <span>查询</span></el-button>
                            </div>
                        </li>
                        <!--<li class="filter-item">
                            <div class="filter-label">企业名称：</div>
                            <div class="filter-con">
                                <el-input style="width: 150px;" v-model="search.entName">

                                </el-input>
                            </div>
                        </li>
                        <li class="filter-item">
                            <el-button type="info" size="small" plain @click="searchEntManagementPlanReport">查询</el-button>
                        </li>-->
                    </ul>
                </div>
                <div class="total-number" style="margin-left: 3px;">
                    <div class="total-name">全部企业{{count.all}}家</div>
                    <div class="total-text">已提交企业<span class="num">{{count.one}}</span>家</div>
                    <div class="total-text is-offline">未提交企业<span class="num">{{count.two}}</span>家</div>
                </div>
                <table class="table-default">
                    <colgroup>
                        <col style="width: 60px;">
                        <col>
                        <col style="width: 8%;">
                        <col style="width: 8%;">
                        <col style="width: 8%;">
                        <col style="width: 8%;">
                        <col style="width: 10%;">
                        <col style="width: 8%;">
                        <col style="width: 8%;">
                        <col style="width: 8%;">
                    </colgroup>
                    <thead>
                    <tr>
                        <th style="text-align:center;width: 6%">序号</th>
                        <th style="text-align:center;width: 10%">企业名称</th>
                        <th style="text-align:center;width: 10%">企业类型</th>
                        <th style="text-align:center;width: 8%">所在地</th>
                        <th style="text-align:center;width: 8%">所属行业</th>
                        <th style="text-align:center;width: 10%">联系人</th>
                        <th style="text-align:center;width: 8%">电话</th>
                        <th style="text-align:center;width: 10%">提交状态</th>
                        <th style="text-align:center;width: 10%">提交时间</th>
                        <th style="text-align:center;width: 10%">操作</th>
                    </tr>
                    </thead>
                    <tbody>
                    <tr v-for="(item,index) in dataList">
                        <td style="text-align:center;width: 6%">{{(cur_page-1)*pageSize+index+1}}</td>
                        <td style="text-align:center;text-overflow: ellipsis;white-space:nowrap;overflow:hidden;width: 10%" :title="item.entName">
                            {{item.entName}}
                        </td>
                        <td style="text-align:center;text-overflow: ellipsis;white-space:nowrap;overflow:hidden;width: 10%" :title="item.entTypeStr">{{item.entTypeStr}}</td>
                        <td style="text-align:center;text-overflow: ellipsis;white-space:nowrap;overflow:hidden;width: 8%" :title="item.regionName">{{item.regionName}}</td>
                        <td style="text-align:center;text-overflow: ellipsis;white-space:nowrap;overflow:hidden;width: 8%" :title="item.categoryName">{{item.categoryName}}</td>
                        <td style="text-align:center;text-overflow: ellipsis;white-space:nowrap;overflow:hidden;width: 10%" :title="item.contacts">{{item.contacts}}</td>
                        <td style="text-align:center;text-overflow: ellipsis;white-space:nowrap;overflow:hidden;width: 8%" :title="item.contactsPhone">{{item.contactsPhone}}</td>
                        <td style="text-align:center;text-overflow: ellipsis;white-space:nowrap;overflow:hidden;width: 10%" >
                            <div v-if="item.submitime" class="text-state">已提交</div>
                            <div v-else class="text-state state-off">未提交</div>
                        </td>
                        <td style="text-align:center;text-overflow: ellipsis;white-space:nowrap;overflow:hidden;width: 10%" :title="item.submitime">
                            <div v-if="item.submitime">{{item.submitime}}</div>
                            <div v-else>--</div>
                        </td>
                        <td style="text-align:center;text-overflow: ellipsis;white-space:nowrap;overflow:hidden;width: 10%" >
                            <div class="btn-opreate">
                                <a v-if="item.submitime" @click="lookEntMonthPeport(item.entId,item.entName)" href="javascript:;" class="text-primary text-underline">查看</a>
                                <!--<a v-else href="javascript:;" class="text-lighter text-underline">查看</a>-->
                            </div>
                        </td>
                    </tr>
                    </tbody>
                </table>
                <div class="el-pagination is-background mt-lg">
                    <span class="el-pagination__total">共 {{total}}  条，每页显示</span>
                    <span class="el-pagination__total">
                                            <el-pagination
                                                    @size-change="handleSizeChange"
                                                    @current-change="handleCurrentChange"
                                                    :current-page="cur_page"
                                                    :page-sizes="pageSizes"
                                                    :page-size="pageSize"
                                                    layout="sizes, prev, pager, next"
                                                    :total="total">
                    </el-pagination>
                    </span>

                    <span class="el-pagination__total">共 {{totalPage}} 页</span>
                </div>
            </div>
        </div>
        <el-dialog
                top="9vh"
                :title="entName"
                :modal="true"
                :modal-append-to-body="false"
                :visible.sync="dialogVisible"
                width="1000px"
                height="50%"
                @close="entReportDialogClose">
            <div>
                <el-tabs :tab-position="tabPosition" v-model="productionName" @tab-click="monthClick">
                    <el-tab-pane v-for="item in yearList"
                                 :key="item.name"
                                 :label="item.name"
                                 :name="item.value"
                    ></el-tab-pane>
                </el-tabs>
            </div>
            <div>
                <div style="font-size: large;color: black;padding-bottom: 12px;">
                    <span style="padding-right: 3px">|</span>危费生产情况
                </div>
                <div style="padding-bottom: 12px;height: 236px;overflow: auto;">
                    <table class="mytable-default" >
                        <thead>
                        <tr>
                            <th style="text-align:center;width: 10%">序号</th>
                            <th style="text-align:center;width: 15%">废物代码</th>
                            <th style="text-align:center;width: 20%">危险废物名称</th>
                            <th style="text-align:center;width: 15%">生产量(吨)</th>
                            <th style="text-align:center;width: 15%">处置利用方式</th>
                            <th style="text-align:center;width: 15%">处置利用数量(吨)</th>
                        </tr>
                        </thead>
                        <tbody>
                        <tr v-for="(item,index) in entMonthOrYearReportInfo.reportEntProductionVoList">
                            <td style="text-align:center;width: 10%">{{(cur_page-1)*pageSize+index+1}}</td>
                            <td style="text-align:center;text-overflow: ellipsis;white-space:nowrap;overflow:hidden;width: 15%" :title="item.dictDetailCode">
                                <!--<a href="javascript:;" class="text-second text-underline">-->
                                {{item.dictDetailCode}}
                                <!--</a>-->
                            </td>
                            <td style="text-align:center;text-overflow: ellipsis;white-space:nowrap;overflow:hidden;width: 20%" :title="item.harmfulName">{{item.harmfulName}}</td>
                            <td style="text-align:center;text-overflow: ellipsis;white-space:nowrap;overflow:hidden;width: 15%" :title="item.productionNumber">{{item.productionNumber}}</td>
                            <td style="text-align:center;text-overflow: ellipsis;white-space:nowrap;overflow:hidden;width: 15%" :title="item.handleType">{{item.handleType}}</td>
                            <td style="text-align:center;text-overflow: ellipsis;white-space:nowrap;overflow:hidden;width: 15%" :title="item.handleNumber">{{item.handleNumber}}</td>
                        </tr>
                        <tr v-if="entMonthOrYearReportInfo.reportEntProductionVoList">
                            <td colspan="3" align="center">
                                合计
                            </td>
                            <td align="center">
                                {{entDialogCountInfo.productionNumber}}
                            </td>
                            <td align="center">
                                /
                            </td>
                            <td align="center">
                                {{entDialogCountInfo.handleNumber}}
                            </td>
                        </tr>
                        </tbody>
                    </table>
                </div>
            </div>
            <div>
                <div style="font-size: large;color: black;padding-bottom: 12px;">
                    <span style="padding-right: 3px">|</span>危费委托利用处置情况
                </div>
                <div style="padding-bottom: 12px;height: 236px;overflow: auto;">
                    <table class="mytable-default" >
                        <thead>
                        <tr>
                            <th style="text-align:center;width: 10%">序号</th>
                            <th style="text-align:center;width: 10%">废物代码</th>
                            <th style="text-align:center;width: 15%">危险废物名称</th>
                            <th style="text-align:center;width: 15%">委托单位名称</th>
                            <th style="text-align:center;width: 10%">所在地</th>
                            <th style="text-align:center;width: 15%">经验许可证编号</th>
                            <th style="text-align:center;width: 15%">处置利用方式</th>
                            <th style="text-align:center;width: 10%">数量(吨)</th>
                        </tr>
                        </thead>
                        <tbody>
                        <tr v-for="(item,index) in entMonthOrYearReportInfo.reportEntEntrusts">
                            <td style="text-align:center;width: 10%">{{(cur_page-1)*pageSize+index+1}}</td>
                            <td style="text-align:center;text-overflow: ellipsis;white-space:nowrap;overflow:hidden;width: 10%" :title="item.dictDetailCode">
                                <!--<a href="javascript:;" class="text-second text-underline">-->
                                {{item.dictDetailCode}}
                                <!--</a>-->
                            </td>
                            <td style="text-align:center;text-overflow: ellipsis;white-space:nowrap;overflow:hidden;width: 15%" :title="item.harmfulName">{{item.harmfulName}}</td>
                            <td style="text-align:center;text-overflow: ellipsis;white-space:nowrap;overflow:hidden;width: 15%" :title="item.client">{{item.client}}</td>
                            <td style="text-align:center;text-overflow: ellipsis;white-space:nowrap;overflow:hidden;width: 10%" :title="item.location">{{item.location}}</td>
                            <td style="text-align:center;text-overflow: ellipsis;white-space:nowrap;overflow:hidden;width: 15%" :title="item.license">{{item.license}}</td>
                            <td style="text-align:center;text-overflow: ellipsis;white-space:nowrap;overflow:hidden;width: 15%" :title="item.handleType">{{item.handleType}}</td>
                            <td style="text-align:center;text-overflow: ellipsis;white-space:nowrap;overflow:hidden;width: 10%" :title="item.entrustNumber">{{item.entrustNumber}}</td>
                        </tr>
                        <tr v-if="entMonthOrYearReportInfo.reportEntEntrusts">
                            <td colspan="7" align="center">
                                合计
                            </td>
                            <td align="center">
                                {{entDialogCountInfo.entrustNumber}}
                            </td>
                        </tr>
                        </tbody>
                    </table>
                </div>
            </div>
            <div>
                <div style="font-size: large;color: black;padding-bottom: 12px;">
                    <span style="padding-right: 3px">|</span>危费贮存情况
                </div>
                <div style="padding-bottom: 12px;height: 236px;overflow: auto;">
                    <table class="mytable-default" >
                        <thead>
                        <tr>
                            <th style="text-align:center;width: 10%">序号</th>
                            <th style="text-align:center;width: 20%">废物代码</th>
                            <th style="text-align:center;width: 20%">危险废物名称</th>
                            <th style="text-align:center;width: 20%">上月底贮存量(吨)</th>
                            <th style="text-align:center;width: 20%">本月底贮存量(吨)</th>
                        </tr>
                        </thead>
                        <tbody>
                        <tr v-for="(item,index) in entMonthOrYearReportInfo.reportEntStorageVos">
                            <td style="text-align:center;width: 10%">{{(cur_page-1)*pageSize+index+1}}</td>
                            <td style="text-align:center;text-overflow: ellipsis;white-space:nowrap;overflow:hidden;width: 20%" :title="item.dictDetailCode">
                                <!--<a href="javascript:;" class="text-second text-underline">-->
                                {{item.dictDetailCode}}
                                <!--</a>-->
                            </td>
                            <td style="text-align:center;text-overflow: ellipsis;white-space:nowrap;overflow:hidden;width: 20%" :title="item.harmfulName">{{item.harmfulName}}</td>
                            <td style="text-align:center;text-overflow: ellipsis;white-space:nowrap;overflow:hidden;width: 20%" :title="item.lastMonth">{{item.lastMonth}}</td>
                            <td style="text-align:center;text-overflow: ellipsis;white-space:nowrap;overflow:hidden;width: 20%" :title="item.thisMonth">{{item.thisMonth}}</td>
                        </tr>
                        <tr v-if="entMonthOrYearReportInfo.reportEntStorageVos">
                            <td colspan="3" align="center">
                                合计
                            </td>
                            <td align="center">
                                {{entDialogCountInfo.lastMonth}}
                            </td>
                            <td align="center">
                                {{entDialogCountInfo.thisMonth}}
                            </td>
                        </tr>
                        </tbody>
                    </table>
                </div>
            </div>
        </el-dialog>
    </div>
</template>

<script>
    export default {
        name: "entYearReport",
        data(){
            return {
                pickerOptions: {
                    disabledDate(time) {
                        // return time.getTime() < Date.now() - 8.64e7;   //禁用以前的日期，今天不禁用
                        // return date.getTime() <= Date.now();    //禁用今天以及以前的日期
                        return time.getTime() > Date.now();
                    }
                },
                isDisabled:false,
                productionName:new Date().format("yyyy"),
                tabPosition: 'top',
                entName:'',
                dialogVisible:false,
                total:0,
                totalPage:0,
                cur_page: 1,
                pageSizes: this.AppConfig.appInfo.pageSizes,
                pageSize: this.AppConfig.appInfo.pageSize,
                regionCode:sessionStorage.getItem('regionCode'),
                regionLevel:this.AppConfig.appInfo.regionLevel,
                parentCode:this.AppConfig.appInfo.parentCode,
                regionName:this.AppConfig.appInfo.regionName,
                entTypeOptions: [{
                    value: '',
                    label: '全部企业'
                }, {
                    value: '1',
                    label: '重点企业'
                }, {
                    value: '2',
                    label: '一般企业'
                },{value: 3,label: "收集企业"},
                    {value: 4,label: "经营企业"}],
                reportStatusOptions:[{
                    value: '',
                    label: '全部'
                }, {
                    value: '1',
                    label: '已备案'
                }, {
                    value: '2',
                    label: '未备案'
                }],
                dialogSearch:{
                    reportYear:new Date().format("yyyy")
                },
                search:{
                    reportYear:new Date().format("yyyy"),
                    regionCode:sessionStorage.getItem('regionCode'),
                    entType:'',
                    standardSituation:'',
                    entName:'',
                    regionName :this.AppConfig.appInfo.regionName,
                    regionLevel:sessionStorage.getItem('regionLevel'),
                    regionParentCode:'',
                    reportStatus:'',
                    year:new Date().format("yyyy"),
                    month:new Date().format("MM"),
                    sourceType:1
                },
                dataList:[],
                count:{
                    all: 0,
                    one: 0,
                    two: 1,
                    three: 0
                },
                regionOptions:[],
                managementPlanFilingEntInfo:'',
                entMonthOrYearReportInfo:'',
                entDialogCountInfo:{
                    productionNumber:0,
                    handleNumber:0,
                    entrustNumber:0,
                    lastMonth:0,
                    thisMonth:0
                },
                entId:'',
                yearList:[{

                }],
                monthList:[{
                    name:"1月",
                    value:"01"
                },{
                    name:"2月",
                    value:"02"
                },{
                    name:"3月",
                    value:"03"
                },{
                    name:"4月",
                    value:"04"
                },{
                    name:"5月",
                    value:"05"
                },{
                    name:"6月",
                    value:"06"
                },{
                    name:"7月",
                    value:"07"
                },{
                    name:"8月",
                    value:"08"
                },{
                    name:"9月",
                    value:"09"
                },{
                    name:"10月",
                    value:"10"
                },{
                    name:"11月",
                    value:"11"
                },{
                    name:"12月",
                    value:"12"
                }]
            }
        },
        created: function () {
            if(this.search.regionLevel==3){
                this.isDisabled = true;
            }
            this.searchEntManagementPlanReport();
            // 行政区域级联
            this.getRegionChildrenList();
            // 初始化弹窗年
            this.initDialogYearList();
        },
        methods:{
            handleSizeChange(val) {
                // 改变每页显示的条数
                this.pageSize=val
                // 注意：在改变每页显示的条数时，要将页码显示到第一页
                this.cur_page=1
                this.pageList();
            },
            handleCurrentChange(val) {
                this.cur_page = val;
                this.pageList();
            },
            getIndex(index){
                let curPage = this.cur_page;
                let limitPage = this.pageSize;
                return (index+1) + (curPage - 1) * limitPage;
            },
            getRegionChildrenList(){
                var _this=this;
                _this.$axios.get('/api/regulatory/regionInfo/getRegionChildrenList',{params:{
                        "regionCode":_this.regionCode
                    }}).then(
                    function (res) {
                        if(res.code == 0){
                            _this.regionOptions = res.data;
                        }
                    }
                ).catch((error) => {
                    // catch 指请求出错的处理
                    console.log(error);
                });
            },
            searchEntManagementPlanReport(){
                this.selectEntMonthYearReportCountEnt();
                this.pageList();
            },
            selectEntMonthYearReportCountEnt(){
                var _this=this;
                var params = new URLSearchParams();
                params.append('regionCode', _this.search.regionCode);
                params.append('year', _this.search.year);
                params.append('month', _this.search.month);
                params.append('sourceType', _this.search.sourceType);

                _this.$axios.get('/api/regulatory/index/selectEntMonthYearReportCountEnt',{params}).then(
                    function (res) {
                        if(res.code == 0){
                            let data = res.data;
                            _this.count.all = data.all;
                            _this.count.one = data.one;
                            _this.count.two = data.two;
                        }
                    }
                ).catch((error) => {
                    // catch 指请求出错的处理
                    console.log(error);
                });
            },
            pageList(){
                var _this=this;
                var params = new URLSearchParams();
                params.append('page', _this.cur_page);
                params.append('limit', _this.pageSize);
                params.append('regionCode', _this.search.regionCode);
                params.append('entType', _this.search.entType);
                params.append('year', _this.search.year);
                params.append('month', _this.search.month);
                params.append('entName', _this.search.entName);
                params.append('sourceType', _this.search.sourceType);
                _this.$axios.get('/api/regulatory/index/selectEntMonthYearReportList',{params}).then(
                    function (res) {
                        if(res.code == 0){
                            _this.dataList = res.data.records;
                            _this.totalPage = res.data.pages;
                            _this.total = res.data.total;
                        }
                    }
                ).catch((error) => {
                    // catch 指请求出错的处理
                    console.log(error);
                });
            },
            searchRegionChange(){
                let regionInfo =this.$refs.regionElCascader.getCheckedNodes();
                if(regionInfo.length>0){
                    this.search.regionName = regionInfo[0].label;
                    this.search.regionLevel= regionInfo[0].level+1;
                    this.search.regionCode = regionInfo[0].value;
                    let parentRegionInfo = regionInfo[0].parent;
                    if(parentRegionInfo!=null){
                        this.search.regionParentCode = parentRegionInfo.value;
                    }else{
                        this.search.regionName = this.regionName;
                        this.search.regionLevel= this.regionLevel;
                        this.search.regionCode = this.regionCode;
                        this.search.regionParentCode = this.parentCode;
                    }
                }else{
                    this.search.regionName = this.regionName;
                    this.search.regionLevel= this.regionLevel;
                    this.search.regionCode = this.regionCode;
                    this.search.regionParentCode = this.parentCode;
                }
                /*console.log(this.search.regionCode+"<<<当前省/市/区");*/
            },
            selectManagementPlanFilingEntInfo(entId){
                var _this=this;
                _this.$axios.get('/api/regulatory/index/selectManagementPlanFilingEntInfo/'+entId).then(
                    function (res) {
                        if(res.code == 0){
                            _this.managementPlanFilingEntInfo = res.data;
                        }
                    }
                ).catch((error) => {
                    // catch 指请求出错的处理
                    console.log(error);
                });
            },
            selectEntMonthYearReportInfo(){
                var _this=this;
                var params = new URLSearchParams();
                params.append('entId', _this.entId);
                params.append('year', _this.search.year);
                params.append('month', '');
                params.append('sourceType', _this.search.sourceType);
                _this.$axios.get('/api/regulatory/index/selectEntMonthYearReportInfo/',{params}).then(
                    function (res) {
                        if(res.code == 0){
                            _this.entMonthOrYearReportInfo = res.data;
                            let reportEntEntrusts = res.data.reportEntEntrusts;
                            let reportEntProductionVoList = res.data.reportEntProductionVoList;
                            let reportEntStorageVos = res.data.reportEntStorageVos;
                            if(reportEntEntrusts.length>0){
                                for (let i = 0; i < reportEntEntrusts.length; i++) {
                                    _this.entDialogCountInfo.entrustNumber+=res.data.reportEntEntrusts[i].entrustNumber;
                                }
                            }
                            if(reportEntProductionVoList.length>0){
                                for (let i = 0; i < reportEntProductionVoList.length; i++) {
                                    _this.entDialogCountInfo.productionNumber+=res.data.reportEntProductionVoList[i].productionNumber;
                                    _this.entDialogCountInfo.handleNumber+=res.data.reportEntProductionVoList[i].handleNumber;
                                }
                            }
                            if(reportEntStorageVos.length>0){
                                for (let i = 0; i < reportEntStorageVos.length; i++) {
                                    _this.entDialogCountInfo.lastMonth+= res.data.reportEntStorageVos[i].lastMonth;
                                    _this.entDialogCountInfo.thisMonth+=res.data.reportEntStorageVos[i].thisMonth;
                                }
                            }
                        }
                    }
                ).catch((error) => {
                    // catch 指请求出错的处理
                    console.log(error);
                });
            },
            lookEntMonthPeport(entId,entName){
                this.entName = entName;
                this.entId = entId;
                this.selectEntMonthYearReportInfo();
                this.dialogVisible = true;
            },
            monthClick(tab, event){
                this.resetEntDialogCountInfo();
                console.log(this.productionName);
                this.search.year = this.productionName;
                /*this.search.month = (parseInt(tab.index)+1);*/
                this.selectEntMonthYearReportInfo();
            },
            entReportDialogClose(){
                this.resetEntDialogCountInfo();
                this.dialogVisible=false;
                this.submitYearChange();
                this.resetEntDialogCountInfo();

            },
            submitYearChange(){
                var year = this.search.reportYear;
                this.dialogSearch.reportYear = year;
                this.search.year = year;
                this.productionName = year;
            },
            dialogSearchReportYearChange(){
                this.resetEntDialogCountInfo();
                this.search.year = this.dialogSearch.reportYear;
                this.selectEntMonthYearReportInfo();
            },
            resetEntDialogCountInfo(){
                this.entDialogCountInfo.productionNumber=0;
                this.entDialogCountInfo.handleNumber=0;
                this.entDialogCountInfo.entrustNumber=0;
                this.entDialogCountInfo.lastMonth=0;
                this.entDialogCountInfo.thisMonth=0;
            },
            initDialogYearList(){
                var yearArr = this.getYearList(3);
                this.yearList = yearArr;
            },

            getYearList(num){
                let thisYear = new Date().format("yyyy");
                if(num==null){
                    return;
                }
                var yearArr = [];
                let thisObj = {};
                thisObj.name=thisYear+"年";
                thisObj.value=thisYear;
                yearArr.push(thisObj);
                for (let i = 1; i <num; i++) {
                    let obj = {};
                    obj.name=(parseInt(thisYear)-i)+"年";
                    obj.value=(parseInt(thisYear)-i)+"";
                    yearArr.push(obj);
                }
                return yearArr.reverse();
            }

        }
    }
</script>

<style>
    .el-divider--vertical {
        display: inline-block;
        width: 3px;
        height: 8em;
        margin: 24px 8px;
        vertical-align: middle;
        position: relative;
    }
    .dabiao{
        width: 70px;height: 20px;background: #5daf34;text-align: center;;
    }
    .jbdabiao{
        width: 70px;height: 20px;background: #f3d9b3;text-align: center;
    }
    .budabiao{
        width: 70px;height: 20px;background: #f56c6c;text-align: center;;
    }

    .mytable-default {
        width: 100%;
        border: 1px solid #c0c4cc;
        color: #222;
    }
    .mytable-default td, .mytable-default th {
        line-height: 22px;
        font-size: 16px;
        border: 1px solid #c0c4cc;
    }
    .mytable-default td:first-child, .mytable-default th:first-child {
        padding-left: 30px;
    }
    .mytable-default tr:nth-child(even) td {
        background-color: #FCFDFF;
    }
    .mytable-default tr:hover td {
        background-color: rgba(15, 162, 245, 0.1);
    }
    .mytable-default th {
        padding: 10px 12px 11px;
        background-color: #F0F2F5;
    }
    .mytable-default td {
        padding: 11px 12px;
        transition: .3s background;
        border-bottom: 1px solid #c0c4cc;
        white-space: nowrap;
    }
    .total-num span {
        font-size: 30px;
        margin-right: 10px;
    }
</style>

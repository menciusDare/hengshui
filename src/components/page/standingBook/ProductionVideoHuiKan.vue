<template>
    <el-dialog :visible.sync="dialogHK" v-if="dialogHK" :title="item.name" :center="true" :modal-append-to-body="false" :close-on-click-modal="false"   top="30px" width="1150px" @close = "closeBtn">
        <div class="modal-bd" style="overflow:hidden;width: 1100px;">
            <!-- 公司信息 -->
            <div class="company-detail">
                <div class="company-title"><img src="../../../../static/css/images/pic/logo01.png">{{item.name}}</div>
                <div class="company-info">地址：<span class="info-text">{{item.address}}</span>注册人：<span class="info-text">{{item.contacts}}</span>手机号：<span class="info-text">{{item.contactsPhone}}</span></div>
            </div>
            <div class="video-section mt">
                <!-- 监控 -->
                <div class="tab-video">
                    <div class="tab-hd">
                        <ul class="tab-head">
                            <li class="is-active"><i class="icon icon-buju" @click="chooseVideoType"></i></li><li style="width: 1px"><!--<i class="icon icon-buju1" @click="chooseVideoType"></i>--></li><li class="ratio-txt" @click="chooseVideoType"><span>16/9</span></li>
                        </ul>
                        <div class="time-now">{{item.monitorTime}}</div>
                    </div>
                    <div class="tab-bd">
                        <div class="video-item is-active">
                            <!-- 左侧车间 -->
                            <div class="video-big">
                                <div class="video-box">
                                    <div class="video-image">
                                        <iframe :src= "videoUrl" width="730px" height="100%" scrolling="no" allowfullscreen="true" webkitallowfullscreen="true" mozallowfullscreen="true"></iframe>
                                    </div>
                                    <div class="video-state">
                                        <a>{{cameraName}}<i class="icon icon-shipin"></i></a>
                                    </div>
                                </div>
                            </div>
                            <!-- 右侧车间 -->
                            <div class="video-auto">
                                <div class="video-auto-box" v-if="videoListNew.length>0">
                                    <div class="video-box" v-for="item in videoListNew">
                                        <div class="video-image">
                                            <iframe :src= "item.url" width="100%" height="100%" scrolling="no" allowfullscreen="true" webkitallowfullscreen="true" mozallowfullscreen="true"></iframe>
                                        </div>
                                        <div class="video-state" @click="videoInfo(item.id)">
                                            <a>{{item.cameraName}}<i class="icon icon-shipin"></i></a>
                                        </div>
                                    </div>
                                </div>
                                <div class="video-auto-box"  v-if="videoListNew.length<=0">
                                    <div class="video-box">
                                        <div class="video-image">
                                            <img src="../../../../static/css/images/pic/video02.jpg" class="full-area">
                                        </div>
                                        <div class="video-state">
                                            <a>贮存设施2<i class="icon icon-shipin"></i></a>
                                        </div>
                                    </div>

                                    <!-- 断网样式.video-break-->
                                    <div class="video-box video-break">
                                        <div class="video-image"></div>
                                        <div class="video-state">
                                            <a>贮存设施3<i class="icon icon-shipin"></i></a>
                                        </div>
                                    </div>
                                </div>
                            </div>
                        </div>
                        <div class="video-item video-average">
                            <div class="video-box" v-for="item in videoList">
                                <div class="video-image">
                                    <iframe :src= "item.url" width="100%" height="100%" scrolling="no" allowfullscreen="true" webkitallowfullscreen="true" mozallowfullscreen="true"></iframe>
                                </div>
                                <div class="video-state" @click="chooseVideo(item.id)">
                                    <a>{{item.cameraName}}<i class="icon icon-shipin"></i></a>
                                </div>
                            </div>
                        </div>
                        <div class="video-item video-ratio">
                            <div class="video-ratio-box"  v-for="item in videoList">
                                <div class="video-box">
                                    <div class="video-image">
                                        <iframe :src= "item.url" width="730px" height="100%" scrolling="no" allowfullscreen="true" webkitallowfullscreen="true" mozallowfullscreen="true"></iframe>
                                    </div>
                                    <div class="video-state"  @click="chooseVideo(item.id)">
                                        <a>{{item.cameraName}}<i class="icon icon-shipin"></i></a>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
                <!-- 回看 -->
                <div class="review-box">
                    <!-- 回看选择时间 -->
                    <div class="panel-box">
                        <div class="panel-hd">
                            <div class="title">日历</div>
                            <!--                            <div class="date-hd">
                                                            <div class="el-date-picker__header"><button type="button" aria-label="上个月" class="el-picker-panel__icon-btn el-date-picker__prev-btn el-icon-arrow-left"></button><span role="button" class="el-date-picker__header-label">2020-05</span><button type="button" aria-label="下个月" class="el-picker-panel__icon-btn el-date-picker__next-btn el-icon-arrow-right"></button></div>
                                                        </div>-->
                        </div>
                        <div class="panel-bd">
                            <div class="date-box">
                                <el-calendar v-model="chooseDateTimes" style="width: 370px">
                                </el-calendar>
                                <div class="date-operate">
                                    <a class="is-active">异常</a>
                                    <a>开视频</a>
                                </div>
                            </div>
                        </div>
                    </div>
                    <!-- 操控台 -->
                    <div class="panel-box mt-sm">
                        <div class="panel-hd">
                            <div class="title">操控台</div>
                        </div>
                        <div class="panel-bd">
                            <div class="operate-area">
                                <!-- 方向 -->
                                <div class="oper-direction">
                                    <!-- 选中样式根据情况需要is-active -->
                                    <div class="direction-cont top" @click="controlVideo(0)"><i class="icon icon-shang"></i></div>
                                    <div class="direction-cont is-active right" @click="controlVideo(3)"><i class="icon icon-you"></i></div>
                                    <div class="direction-cont bottom" @click="controlVideo(1)"><i class="icon icon-xia"></i></div>
                                    <div class="direction-cont left" @click="controlVideo(2)"><i class="icon icon-zuo"></i></div>
                                </div>
                                <!-- 调节 -->
                                <div class="oper-adjust">
<!--                                    <div class="slide-box">
                                        <div class="slide-name">光圈</div>
                                        <div role="slider" aria-valuemin="0" aria-valuemax="100" aria-orientation="horizontal" class="el-slider" aria-valuetext="36" aria-label="slider between 0 and 100">&lt;!&ndash;&ndash;&gt;
                                            <el-button icon="el-icon-minus" @click="decrease"  round></el-button>
                                            <div class="el-slider__runway" @click="controlVideoNew(1)">
                                                <div class="el-slider__bar" :style="guanquan1"></div>
                                                <div tabindex="0" class="el-slider__button-wrapper" :style="guanquan2">
                                                    <div class="el-tooltip el-slider__button" aria-describedby="el-tooltip-8216" tabindex="0" ></div>
                                                </div>&lt;!&ndash;&ndash;&gt;&lt;!&ndash;&ndash;&gt;
                                            </div>
                                            <el-button icon="el-icon-plus" @click="increase" round></el-button>
                                        </div>
                                        <div class="slide-text">{{guanquan}}</div>
                                    </div>-->
                                    <div class="slide-box">
                                        <div class="slide-name">焦距</div>
                                        <div role="slider" aria-valuemin="0" aria-valuemax="100" aria-orientation="horizontal" class="el-slider" aria-valuetext="36" aria-label="slider between 0 and 100"><!---->
                                            <!--</div>
                                                <div tabindex="0" class="el-slider__button-wrapper" :style="jiaoju2">
                                                    <div class="el-tooltip el-slider__button" aria-describedby="el-tooltip-8216" tabindex="0"></div>
                                                </div>
                                            </div>-->
                                            <div style="width: 50px">
                                                <el-slider v-model="jiaoju" :show-tooltip="false" @change="chooseJiaoJu"></el-slider>
                                            </div>
                                        </div>
                                        <div class="slide-text">{{parseInt(jiaoju*0.28)}}mm</div>
                                    </div>
                                    <div class="slide-box">
                                        <div class="slide-name">缩放</div>
                                      <div role="slider" aria-valuemin="0" aria-valuemax="100" aria-orientation="horizontal" class="el-slider" aria-valuetext="36" aria-label="slider between 0 and 100">
                                          <!--    <div class="el-slider__runway">
                                                <div class="el-slider__bar" :style="suofang1"></div>
                                                <div tabindex="0" class="el-slider__button-wrapper" :style="suofang2">
                                                    <div class="el-tooltip el-slider__button" aria-describedby="el-tooltip-8216" tabindex="0"></div>
                                                </div>&lt;!&ndash;&ndash;&gt;&lt;!&ndash;&ndash;&gt;
                                            </div>
                                        </div>-->
                                        <div style="width: 50px">
                                            <el-slider v-model="suofang" :show-tooltip="false"  @change="chooseSuoFang"></el-slider>
                                        </div>
                                    </div>
                                        <div class="slide-text">{{ parseInt(suofang*0.28)}}mm</div>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
            <!-- 报警信息 -->
            <div class="panel-box mt-sm">
                <div class="panel-hd">
                    <div class="title">报警信息</div>
                </div>
                <div class="panel-bd">
                    <div class="small-hei" style="height: 126px;text-align: center">
                        <table class="table table-sm">
                            <thead>
                            <tr>
                                <th class="text-center">序号</th>
                                <th>时间</th>
                                <th>消息</th>
                            </tr>
                            </thead>
                            <tbody>
                            <tr v-for="itme in shiList" v-if="shiList.length>0">
                                <td class="text-center">{{itme.id}}</td>
                                <td>{{itme.createTime}}</td>
                                <td :title="itme.message">{{itme.message}}</td>
                            </tr>
                            <tr v-if="shiList.length<=0">
                                <td colspan="3">暂无数据</td>
                            </tr>
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>
        </div>
    </el-dialog>
</template>

<script>
    export default {
        name: "productionVideo",
        components:{
        },
        data(){
            return{
                dateArray:[],
                activeName:7,
                chooseDate:"",
                statTime:'',
                endTime:'',
                current:0,
                entAddress:"",
                legalPhone:"",
                entName:"",
                contacts:'',
                videoList:[],
                videoUrl:'',
                videoAdress:"",
                dialogHK:false,
                entId:"",
                showWH:"",
                videoType:'',
                deviceNo:'',
                shiList:[],
                shitTime:"",
                item:{},
                chooseDateTimes: null,
                type: 1,
                cameraName:'',
                path: '/api/regulatory/',
                videoListNew:[],
                videoId: '',
                jiaoju: 0,
                jiaojuCope: 0,
                suofang:0,
                suofangCope:0,
                statTime1:'',
                endTime1:'',

            }
        },
        methods:{
            chooseJiaoJu(){
                var _this = this;
                if(_this.jiaojuCope > _this.jiaoju){
                    _this.controlVideo(10)
                }else if(_this.jiaojuCope < _this.jiaoju){
                    _this.controlVideo(11)
                }
                _this.jiaojuCope = _this.jiaoju;
            },
            chooseSuoFang(){
                var _this = this;
                if(_this.suofangCope > _this.suofang){
                    _this.controlVideo(9)
                }else if(_this.suofangCope < _this.suofang){
                    _this.controlVideo(8)
                }
                _this.suofangCope = _this.suofang;
            },
            chooseVideoType(){
                this.videoId = '';
            },
            chooseVideo(id){
                this.videoId = id;
            },
            closeBtn(){
                this.type = 1;
                this.entId = "";
                this.entAddress= "";
                this.legalPhone= "";
                this.entName= "";
                this.contacts= '';
                this.videoList= [];
                this.videoListNew = [];
                this.videoUrl= "";
                this.videoAdress= "";
                this.pageType= "";
                this.cffalg=false;
                this.qtfalg=false;
                this.deviceNo="";
                this.videoType="";
                this.shiList= [];
                this.shitTime= "";
                this.statTime1 = "";
                this.endTime1="";
                this.chooseDateTimes = null;
                this.item= {};
                this.dialogHK=false;

            },
            getDateList(){
                var _this = this;
                if(_this.chooseDateTimes!=null){
                    var dateTime = _this.chooseDateTimes.format("yyyyMMdd");
                    _this.statTime = dateTime+"0000"
                    _this.endTime = dateTime+"2359"
                    _this.statTime1 = _this.chooseDateTimes.format("yyyy-MM-dd")+" 00:00:00"
                    _this.endTime1 = _this.chooseDateTimes.format("yyyy-MM-dd")+" 23:59:59"
                    _this.type = 2;
                }else{
                    _this.type = 1;
                }

            },
            loadingData() {
                var _this = this;
                _this.$nextTick(() => {
                    var url = _this.path +'tVideocamera/queryVideoList';
                    var params = new URLSearchParams();
                    _this.getDateList();
                    params.append('starDate',  _this.statTime);
                    params.append('endDate',  _this.endTime);
                    params.append('type',_this.type);
                    params.append('entId', _this.item.entId);
                    _this.$axios.post(
                        url, params
                    ).then((response) => {
                        if(response.code == 0) {
                            _this.entAddress = response.data.entAddress;
                            _this.legalPhone = response.data.legalPhone;
                            _this.entName = response.data.entName;
                            _this.contacts = response.data.contacts;
                            _this.videoInfo(response.data.data[0].id)
                            _this.videoList = response.data.data;
                            var num = this.videoList.length;//16*9视频个数
                            $(".video-ratio-box").width(10000);
                            this.showWH = 'width:' + num * 1040 + 'px;';
                        }
                    }).catch((error) => {
                        // catch 指请求出错的处理
                        console.log(error);
                    });
                })
            },

            controlVideo(direction) {
                var _this = this;
                var url = _this.path +'tVideocamera/getVideoControl';
                var params = new URLSearchParams();
                params.append('direction',direction);
                params.append('id', _this.videoId);
                if(_this.videoId!=null&&_this.videoId!=''){
                    _this.$axios.post(
                        url, params
                    ).then((response) => {
                        if(response.code == 0) {
                            _this.$message.success("操控成功!")
                        }else{
                            _this.$message.error(response.msg)
                        }
                    }).catch((error) => {
                        // catch 指请求出错的处理
                        console.log(error);
                    })
                }else{
                    _this.$message.error("请选择需要控制的摄像头!")
                }
            },
            videoInfo(id){
                var _this = this;
                _this.showbig=false;
                _this.$nextTick(() => {
                    var url = _this.path +'tVideocamera/queryVideoInfo';
                    var params = new URLSearchParams();
                    _this.getDateList();
                    params.append('starDate',  _this.statTime);
                    params.append('endDate',  _this.endTime);
                    params.append('type',_this.type);
                    params.append('entId', id);
                    _this.$axios.post(
                        url, params
                    ).then((response) => {
                        if(response.code == 0) {
                            _this.videoUrl = response.data.url;
                            _this.videoAdress = response.data.urlAdress;
                            _this.cameraName = response.data.cameraName;
                            _this.videoListNew = [];
                            _this.videoId = "";
                            _this.videoId = response.data.id;
                            for(var i = 0 ; i< _this.videoList.length;i++){
                                if(response.data.id != _this.videoList[i].id){
                                    _this.videoListNew.push(_this.videoList[i])
                                }
                            }
                            _this.showbig = true;
                        }
                    }).catch((error) => {
                        // catch 指请求出错的处理
                        console.log(error);
                    });
                })
            },
            selectInfoList(){
                var _this = this;
                _this.$nextTick(() => {
                    _this.getDateList();
                    var url =  _this.path + 'tVideocamera/queryAlarmInformationPage?entId='+_this.item.entId+"&starDate="+_this.statTime1+"&endDate="+_this.endTime1;
                    _this.$axios.get(
                        url
                    ).then((response) => {
                        if(response.code == 0) {
                            _this.shiList = [];
                            for (var i = 0; i < response.data.records.length; i++) {
                                var param = {
                                    "id": i + 1,
                                    "createTime": response.data.records[i].createTime,
                                    "message": response.data.records[i].message
                                }
                                _this.shiList.push(param);
                            }
                        }
                    }).catch((error) => {
                        // catch 指请求出错的处理
                        console.log(error);
                    });
                })
            },
            show(){
                var _this = this;
                _this.loadingData();
                _this.selectInfoList();
                _this.type = 1;
            },
            openVideo(item){
                var _this = this;
                _this.$nextTick(() => {
                    var oJs = document.createElement('script');
                    oJs.setAttribute("type", "text/javascript");
                    oJs.setAttribute("src", "static/css/js/common.js");
                    document.getElementsByTagName("head")[0].appendChild(oJs); //绑定
                    _this.item = item;
                    _this.dialogHK=true;
                    _this.show();
                })
            },
        },
        watch:{
            chooseDateTimes () {
                var _this = this;
                var star = _this.chooseDateTimes.format("yyyyMMdd");
                var end = new Date().format("yyyyMMdd");
                if(star>end){
                    var msg = _this.chooseDateTimes.format("yyyy年MM月dd日");
                    if(_this.chooseDateTimes != null){
                        _this.chooseDateTimes = new Date();
                    }
                    _this.$message.error("请选择"+msg+"及之前的时间!")
                }else{
                    this.show();
                }

            },

        },
        created: function () {

        },
    }
</script>

<style scoped>

</style>

<template>
  <div class="main">
    <div class="main-top">
      <div class="breadcrumb">
        <a href="#">
          <i class="icon icon-zhuanyun"></i>
          <span class="text-primary-font">经营企业监管</span>
        </a>
        <span class="sep">&gt;</span>
        <a href="#">
          <i class="icon icon-zhuanyun"></i>
          <span class="text-primary-font">企业台账监控</span>
        </a>
      </div>
    </div>
    <div class="panel">
      <div class="panel-bd pdtlr-0">
        <div class="breadcrumb" style="width: 95%">
          <span
            class="fenge"
            style="
              padding: 12px 6px 15px 6px;
              margin-left: 6px;
              border-left: 1px solid #000;
            "
          ></span>

          <span>
            <img
              src="../../../../static/css/images/pic/logo02.png"
              style="margin-bottom: -20px; margin-right: 8px"
            />
          </span>
          <span class="text-primary-font" style="font-size: 24px">{{
            entName
          }}</span>
        </div>
      </div>

      <div class="main-container">
        <el-tabs v-model="activeName" @tab-click="handleClick">
          <el-tab-pane label="贮存台账" name="second" v-if="true">
            <table class="table-default" v-if="true">
              <thead>
                <tr>
                  <th style="text-align: center; width: 6%">序号</th>
                  <th style="text-align: center; width: 14%">操作时间</th>
                  <th style="text-align: center; width: 14%">操作类型</th>
                  <th style="text-align: center; width: 10%">危废名称</th>
                  <th style="text-align: center; width: 10%">危废代码</th>
                  <th style="text-align: center; width: 10%">危废类别</th>
                  <th style="text-align: center; width: 10%">数量(KG)</th>
                  <th style="text-align: center; width: 10%">贮存容器</th>
                  <th style="text-align: center; width: 10%">批次</th>
                  <th style="text-align: center; width: 10%">经办人</th>
                  <th style="text-align: center; width: 10%">来源部门</th>
                  <th style="text-align: center; width: 10%">操作</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="(item, i) in tableDatas1" :key="i">
                  <td style="text-align: center; width: 6%">
                    {{ pageSize * (cur_page - 1) + i + 1 }}
                  </td>
                  <td
                    style="
                      text-align: center;
                      text-overflow: ellipsis;
                      white-space: nowrap;
                      overflow: hidden;
                      width: 14%;
                    "
                    :title="item.storageTime"
                  >
                    {{ item.storageTime }}
                  </td>
                  <td
                    style="
                      text-align: center;
                      text-overflow: ellipsis;
                      white-space: nowrap;
                      overflow: hidden;
                      width: 10%;
                    "
                    :title="item.operationType"
                  >
                    {{
                      item.operationType
                        ? item.operationType == 1
                          ? "入库"
                          : item.operationType == 2
                          ? "出库"
                          : ""
                        : null
                    }}
                  </td>
                  <td
                    style="
                      text-align: center;
                      text-overflow: ellipsis;
                      white-space: nowrap;
                      overflow: hidden;
                      width: 10%;
                    "
                    :title="item.dictDetailName"
                  >
                    {{ item.dictDetailName }}
                  </td>
                  <td
                    style="
                      text-align: center;
                      text-overflow: ellipsis;
                      white-space: nowrap;
                      overflow: hidden;
                      width: 10%;
                    "
                    :title="item.dictDetailCode"
                  >
                    {{ item.dictDetailCode }}
                  </td>
                  <td
                    style="
                      text-align: center;
                      text-overflow: ellipsis;
                      white-space: nowrap;
                      overflow: hidden;
                      width: 10%;
                    "
                    :title="item.dictTypeName"
                  >
                    {{ item.dictTypeName }}
                  </td>
                  <td
                    style="
                      text-align: center;
                      text-overflow: ellipsis;
                      white-space: nowrap;
                      overflow: hidden;
                      width: 10%;
                    "
                    :title="item.number"
                  >
                    {{ item.number }}
                  </td>
                  <td
                    style="
                      text-align: center;
                      text-overflow: ellipsis;
                      white-space: nowrap;
                      overflow: hidden;
                      width: 10%;
                    "
                    :title="item.containerName"
                  >
                    {{ item.containerName }}
                  </td>
                  <td
                    style="
                      text-align: center;
                      text-overflow: ellipsis;
                      white-space: nowrap;
                      overflow: hidden;
                      width: 10%;
                    "
                    :title="item.containerNumber"
                  >
                    {{item.containerNumber}}
                  </td>
                  <td
                    style="
                      text-align: center;
                      text-overflow: ellipsis;
                      white-space: nowrap;
                      overflow: hidden;
                      width: 10%;
                    "
                    :title="item.userName"
                  >
                    {{ item.userName }}
                  </td>
                   <td
                    style="
                      text-align: center;
                      text-overflow: ellipsis;
                      white-space: nowrap;
                      overflow: hidden;
                      width: 10%;
                    "
                    :title="item.deptName"
                  >
                    {{ item.deptName }}
                  </td>
                  <td style="text-align: center; width: 10%">
                    <a
                      href="javascript:;"
                      class="text-primary text-underline"
                      @click="details(item, 2)"
                      >查看</a
                    >
                    <!-- <a
                          v-if="item.operationType==1"
                          href="javascript:;"
                          class="text-primary text-underline"
                          @click="printProBook(item)"
                  >打印</a>-->
                  </td>
                </tr>
              </tbody>
            </table>
          </el-tab-pane>
          <el-tab-pane label="自行利用处置台账" name="four" v-if="true">
            <table class="table-default" v-if="true">
              <thead>
                <tr>
                  <th style="text-align: center; width: 6%">序号</th>
                  <th style="text-align: center; width: 14%">操作时间</th>
                  <th style="text-align: center; width: 10%">危废名称</th>
                  <th style="text-align: center; width: 10%">危废代码</th>
                  <th style="text-align: center; width: 10%">危废类别</th>
                  <th style="text-align: center; width: 10%">数量(KG)</th>
                  <th style="text-align: center; width: 10%">贮存容器</th>
                  <th style="text-align: center; width: 10%">容器个数</th>
                  <th style="text-align: center; width: 10%">利用处置方式</th>
                  <th style="text-align: center; width: 10%">完成时间</th>
                  <th style="text-align: center; width: 10%">经办人</th>
                  <th style="text-align: center; width: 10%">溯源码</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="(item, index) in tableDatas2" :key="index">
                  <td style="text-align: center; width: 6%">
                    {{ pageSize * (cur_page - 1) + index + 1 }}
                  </td>
                  <td
                    style="
                      text-align: center;
                      text-overflow: ellipsis;
                      white-space: nowrap;
                      overflow: hidden;
                      width: 14%;
                    "
                    :title="item.acceptTime"
                  >
                    {{ item.acceptTime }}
                  </td>
                  <td
                    style="
                      text-align: center;
                      text-overflow: ellipsis;
                      white-space: nowrap;
                      overflow: hidden;
                      width: 10%;
                    "
                    :title="item.dictDetailName"
                  >
                    {{ item.dictDetailName }}
                  </td>
                  <td
                    style="
                      text-align: center;
                      text-overflow: ellipsis;
                      white-space: nowrap;
                      overflow: hidden;
                      width: 10%;
                    "
                    :title="item.dictDetailCode"
                  >
                    {{ item.dictDetailCode }}
                  </td>
                  <td
                    style="
                      text-align: center;
                      text-overflow: ellipsis;
                      white-space: nowrap;
                      overflow: hidden;
                      width: 10%;
                    "
                    :title="item.dictTypeName"
                  >
                    {{ item.dictTypeName }}
                  </td>
                  <td
                    style="
                      text-align: center;
                      text-overflow: ellipsis;
                      white-space: nowrap;
                      overflow: hidden;
                      width: 10%;
                    "
                    :title="item.number"
                  >
                    {{ item.number }}
                  </td>
                  <td
                    style="
                      text-align: center;
                      text-overflow: ellipsis;
                      white-space: nowrap;
                      overflow: hidden;
                      width: 10%;
                    "
                    :title="item.containerName"
                  >
                    {{ item.containerName }}
                  </td>
                  <td
                    style="
                      text-align: center;
                      text-overflow: ellipsis;
                      white-space: nowrap;
                      overflow: hidden;
                      width: 10%;
                    "
                    :title="item.containerNumber"
                  >
                    1
                  </td>
                  <td
                    style="
                      text-align: center;
                      text-overflow: ellipsis;
                      white-space: nowrap;
                      overflow: hidden;
                      width: 10%;
                    "
                    :title="item.mode"
                  >
                    {{ item.mode }}
                  </td>
                  <td
                    style="
                      text-align: center;
                      text-overflow: ellipsis;
                      white-space: nowrap;
                      overflow: hidden;
                      width: 10%;
                    "
                    :title="item.completeTime"
                  >
                    {{ item.completeTime }}
                  </td>
                  <td
                    style="
                      text-align: center;
                      text-overflow: ellipsis;
                      white-space: nowrap;
                      overflow: hidden;
                      width: 10%;
                    "
                    :title="item.userName"
                  >
                    {{ item.userName }}
                  </td>
                  <td style="text-align: center; width: 10%">
                    <a
                      href="javascript:;"
                      class="text-primary text-underline"
                      @click="details(item, 4)"
                      >查看</a
                    >
                  </td>
                </tr>
              </tbody>
            </table>
          </el-tab-pane>
        </el-tabs>
        <div class="el-pagination is-background mt-lg">
          <span class="el-pagination__total"
            >共 {{ total }} 条，每页显示</span
          >
          <span class="el-pagination__total">
            <el-pagination
              @size-change="handleSizeChange"
              @current-change="handleCurrentChange"
              :current-page="cur_page"
              :page-sizes="pageSizes"
              :page-size="pageSize"
              layout="sizes, prev, pager, next"
              :total="total"
            ></el-pagination>
          </span>

          <span class="el-pagination__total">共 {{ totalPage }} 页</span>
        </div>
      </div>
      <el-dialog
        title="订单流转信息"
        :visible.sync="dialogVisible"
        class="dlogBox"
        :modal-append-to-body="false"
        width="60%"
      >
        <div>
          <!-- <a href="javascript:;" class="modal-close icon icon-close" title="关闭"></a>
          <div class="modal-hd">
            <h2 class="modal-tit">订单流转信息</h2>
          </div>-->
          <div class="modal-bd">
            <!-- 危废 -->
            <div class="wf-info">
              <div class="wf-hd">
                <div class="wf-time">
                  <i class="icon icon-time"></i>
                  {{ tableData.time }}
                </div>
              </div>
              <div class="wf-bd">
                <table class="table-text">
                  <tr>
                    <td>
                      <div class="list-name">危废名称：</div>
                      <div class="list-text">
                        {{ tableData.dictDetailName }}
                      </div>
                    </td>
                    <td>
                      <div class="list-name">数量(KG)：</div>
                      <div class="list-text">{{ tableData.number }}</div>
                    </td>
                    <td>
                      <div class="list-name">贮存容器：</div>
                      <div class="list-text">{{ tableData.containerName }}</div>
                    </td>
                    <td>
                      <div class="list-name">批次：</div>
                      <div class="list-text">{{tableData.containerNumber}}</div>
                    </td>
                  </tr>
                  <tr>
                    <td>
                      <div class="list-name">危废类别：</div>
                      <div class="list-text">{{ tableData.dictTypeName }}</div>
                    </td>
                    <td>
                      <div class="list-name">危废代码：</div>
                      <div class="list-text">
                        {{ tableData.dictDetailCode }}
                      </div>
                    </td>
                    <td>
                      <div class="list-name">经办人：</div>
                      <div class="list-text">{{ tableData.userName }}</div>
                    </td>
                    <td v-if="tableData.type == 4">
                      <div class="list-name">完成时间：</div>
                      <div class="list-text">{{ tableData.completeTime }}</div>
                    </td>
                  </tr>
                </table>
              </div>
            </div>
            <!-- 处置 -->
            <div class="cz-cont mt">
              <div class="code-box">
                <img
                  :src="
                    tattachment
                      ? imgurl + tattachment.locationUrl
                      : '../../../../static/css/images/code.png'
                  "
                  alt
                  class="code-pic"
                />
                <div class="code-text">
                  {{ tableData.sourceTracing
                  }}<!--预留相关内容文字描述放置两行-->
                </div>
              </div>
              <div class="step-box step03">
                <ol class="step-list">
                  <li class="is-active" style="height: unset" v-if="cz">
                    <div class="step-time">
                      <span class="time-text">利用处置</span>
                    </div>
                    <div class="step-icon">
                      <span class="step-num"></span>
                      <div class="step-line" style="height: unset"></div>
                    </div>
                    <div class="qiye-con">
                      <div class="qiye-item">
                        <div>处置企业：</div>
                        <div>{{ regionName }} {{ entName }}</div>
                      </div>
                      <div class="qiye-item">
                        <div>处置时间：</div>
                        <div>{{ cz.time }}</div>
                      </div>
                    </div>
                  </li>
                  <li
                    :class="cz ? '' : 'is-active'"
                    style="height: unset"
                    v-if="zy"
                  >
                    <div class="step-time">
                      <span class="time-text">转移</span>
                    </div>
                    <div class="step-icon">
                      <span class="step-num"></span>
                      <div class="step-line" style="height: unset"></div>
                    </div>
                    <div class="qiye-con">
                      <div class="qiye-item">
                        <div>承运企业：</div>
                        <div>{{ zy.regionName }} {{ zy.entName }}</div>
                      </div>
                      <div class="qiye-item">
                        <div>车辆：</div>
                        <div>{{ zy.cardNo }}</div>
                      </div>
                      <div class="qiye-item">
                        <div>承运人：</div>
                        <div>{{ zy.contacts }}</div>
                      </div>
                      <div class="qiye-item">
                        <div>运输时间：</div>
                        <div>{{ zy.time }}</div>
                      </div>
                      <div class="qiye-item">
                        <div>五联单：</div>
                        <div>{{ zy.fivefoldSheet }}</div>
                      </div>
                    </div>
                  </li>

                  <li
                    :class="cz || zy ? '' : 'is-active'"
                    style="height: unset"
                    v-if="zc"
                  >
                    <div class="step-time">
                      <span class="time-text">贮存</span>
                    </div>
                    <div class="step-icon">
                      <span class="step-num"></span>
                      <div class="step-line" style="height: unset"></div>
                    </div>
                    <div class="qiye-con">
                      <div class="qiye-item">
                        <div>贮存企业：</div>
                        <div>{{ regionName }} {{ entName }}</div>
                      </div>
                      <div class="qiye-item">
                        <div>接收时间：</div>
                        <div>{{ zc.time }}</div>
                      </div>
                    </div>
                  </li>
                  <li
                    :class="cz || zy || zc ? '' : 'is-active'"
                    style="height: unset"
                    v-if="sc"
                  >
                    <div class="step-time">
                      <span class="time-text">生产</span>
                    </div>
                    <div class="step-icon">
                      <span class="step-num"></span>
                      <div class="step-line" style="height: unset"></div>
                    </div>
                    <div class="qiye-con">
                      <div class="qiye-item">
                        <div>生产企业：</div>
                        <div>{{ regionName }} {{ entName }}</div>
                      </div>
                      <div class="qiye-item">
                        <div>接收时间：</div>
                        <div>{{ sc.time }}</div>
                      </div>
                    </div>
                  </li>
                </ol>
              </div>
            </div>
          </div>
        </div>
      </el-dialog>
    </div>
  </div>
</template>

<script>
import downloadFile from "../../common/unit/DownloadFile";
export default {
  name: "RegisterHome",
  data() {
    return {
      typeNameList: [],
      typeId: "",
      type: "",
      cur_page: 1,
      pageSizes: this.AppConfig.appInfo.pageSizes,
      pageSize: this.AppConfig.appInfo.pageSize,
      tableDatas1: [],
      tableDatas2: [],
      total: 0,
      dialogVisible: false,
      totalPage: 0,
      entId: "",
      // entId: this.$route.query.entId,
      activeName: "second",
      entName: "",
      // entName: this.$route.query.entName,
      // regionName: this.$route.query.regionName,
      regionName: "",
      port: "/api/regulatory/",
      //  port: "http://localhost:8006/",
      tableDatas: [],
      tableData: {},
      imgurl: this.IMG_URL,
      zc: null,
      cz: null,
      sc: null,
      zy: null,
      tattachment: null,
      YearOptions: [
        {
          value: "",
          label: "全部年份",
        },
        {
          value: "2019",
          label: "2019",
        },
        {
          value: "2020",
          label: "2020",
        },
      ],
    };
  },
  // mounted() {
  //   this.selectentErpriseSSBook();
  // },
  created() {
    this.getTypeNameList();
    this.getInfo();
  },
  methods: {
    handleChange(value) {
      if (value.length == 1) {
        this.type = 1;
      } else if (value.length == 2) {
        this.type = 2;
      } else if (value.length == 0) {
        this.type = "";
      }
      if (value.length > 0) {
        this.typeId = value[value.length - 1];
      } else {
        this.typeId = "";
      }
    },
    getTypeNameList() {
      var _this = this;
      _this.$axios
        .get("/api/regulatory/type/getTypeNameLists/" + 1)
        .then((res) => {
          if (res.code == 0 && res.data != null) {
            _this.typeNameList = res.data;
          }
        })
        .catch(function (err) {
          console.log(err);
        });
    },
    printProBook(item) {
      this.$confirm("是否确认打印?", "提示", {
        confirmButtonText: "确定",
        cancelButtonText: "取消",
        closeOnClickModal: false,
        type: "warning",
      })
        .then(() => {
          this.getPrinting(item);
        })
        .catch(() => {
          this.$message({
            type: "info",
            message: "已取消打印",
          });
        });
    },
    getPrinting(item) {
      let _this = this;
      if (item.storageId == null) {
        _this.$message.info("数据错误！");
        return;
      }
      /* var reqItem = {};
       reqItem.messageType = "5";
       reqItem.data = item.number;
       reqItem.hazardousName = item.dictDetailName;
       reqItem.barCode = item.sourceTracing;
       reqItem.status = 0;
       reqItem.storageId = item.storageId;
       reqItem.hazardousType = item.dictTypeName;*/
      let params = new URLSearchParams();
      params.append("messageType", "5");
      params.append("data", item.number);
      params.append("hazardousName", item.dictDetailName);
      params.append("barCode", item.sourceTracing);
      params.append("status", 0);
      params.append("storageId", item.storageId);
      params.append("hazardousType", item.dictTypeName);
      params.append("newDate", item.storageTime);
      _this.$axios
        .post("/api/enterprise/terminal/printing/executePrinting", params)
        .then((res) => {
          if (res.code == 0) {
            _this.$message.success("打印成功");
          } else {
            _this.$message.error(res.msg);
          }
        })
        .catch(function (err) {
          console.log(err);
        });
    },
    exportExcel() {
      let _this = this;
      let params = new URLSearchParams();
      params.append("page", _this.cur_page);
      params.append("size", _this.pageSize);
      params.append("entId", _this.entId);
      params.append("typeId", _this.typeId);
      params.append("activeName", _this.activeName);
      params.append("type", _this.type);
      _this.$axios
        .post("/api/regulatory/tEnterpriseInfo/export", params, {
          responseType: "blob",
        })
        .then((res) => {
          downloadFile(res);
        })
        .catch(function (err) {
          console.log(err);
        });
    },
    getInfo() {
      let self = this;
      self.$axios
        .get("/api/regulatory/tVideocamera/getEntIdAndCompanyName")
        .then((res) => {
          if (res.code == 0 && res.data != null) {
            (self.entId = res.data.id), (self.entName = res.data.name);
            self.regionName = res.data.regionCode;
            this.selectentErpriseSSBook();
          }
        })
        .catch(function (err) {
          console.log(err);
        });
    },
    handleClick(tab, event) {
      this.cur_page = 1;
      this.pageSize = 10;
      console.log("tab.name", tab.name);
      this.init(tab.name);
    },
    init(name) {
      if (name == "first") {
        this.selectentErprisePSBook();
      } else if (name == "second") {
        this.selectentErpriseSSBook();
      } else if (name == "third") {
        this.selectentErpriseTSBook();
      } else if (name == "four") {
        this.selectentErpriseVSBook();
      }
    },
    search() {
      if (this.activeName == "second") {
        this.selectentErpriseSSBook();
      } else if (this.activeName == "third") {
        this.selectentErpriseTSBook();
      } else if (this.activeName == "four") {
        this.selectentErpriseVSBook();
      } else if (this.activeName == "first") {
        this.selectentErprisePSBook();
      }
    },
    //企业生产台账
    selectentErprisePSBook() {
      var _this = this;
      _this.$axios
        .post(_this.port + "tEnterpriseInfo/selectentErprisePSBook", {
          page: _this.cur_page,
          limit: _this.pageSize,
          entId: _this.entId,
          typeId: _this.typeId,
          type: _this.type,
        })
        .then((response) => {
          if (response.code == 0) {
            // _this.tableDatas = response.data.records;
            _this.tableDatas = JSON.parse(JSON.stringify(response.data.records)); 
            // _this.total = response.data.total;
            _this.total = JSON.parse(JSON.stringify(response.data.total));
            // _this.totalPage = response.data.pages;
            _this.totalPage = JSON.parse(JSON.stringify(response.data.pages));
          }
        })
        .catch((error) => {
          // catch 指请求出错的处理
          console.log(error);
        });
    },
    //企业贮存台账
    selectentErpriseSSBook() {
      var _this = this;
      _this.$axios
        .post(_this.port + "tEnterpriseInfo/selectentErpriseSSBook", {
          page: _this.cur_page,
          limit: _this.pageSize,
          entId: _this.entId,
          typeId: _this.typeId,
          type: _this.type,
        })
        .then((response) => {
          if (response.code == 0) {
             // _this.tableDatas = response.data.records;
            _this.tableDatas1 = JSON.parse(JSON.stringify(response.data.records)); 
            // _this.total = response.data.total;
            _this.total = JSON.parse(JSON.stringify(response.data.total));
            // _this.totalPage = response.data.pages;
            _this.totalPage = JSON.parse(JSON.stringify(response.data.pages));
          }
        })
        .catch((error) => {
          // catch 指请求出错的处理
          console.log(error);
        });
    },
    //企业转移台账
    selectentErpriseTSBook() {
      var _this = this;
      _this.$axios
        .post(_this.port + "tEnterpriseInfo/selectentErpriseTSBook", {
          page: _this.cur_page,
          limit: _this.pageSize,
          entId: _this.entId,
          typeId: _this.typeId,
          type: _this.type,
        })
        .then((response) => {
          if (response.code == 0) {
             // _this.tableDatas = response.data.records;
            _this.tableDatas = JSON.parse(JSON.stringify(response.data.records)); 
            // _this.total = response.data.total;
            _this.total = JSON.parse(JSON.stringify(response.data.total));
            // _this.totalPage = response.data.pages;
            _this.totalPage = JSON.parse(JSON.stringify(response.data.pages));
          }
        })
        .catch((error) => {
          // catch 指请求出错的处理
          console.log(error);
        });
    },
    //企业自行处置台账
    selectentErpriseVSBook() {
      var _this = this;
      _this.$axios
        .post(_this.port + "tEnterpriseInfo/selectentErpriseVSBook", {
          page: _this.cur_page,
          limit: _this.pageSize,
          entId: _this.entId,
          typeId: _this.typeId,
          type: _this.type,
        })
        .then((response) => {
          if (response.code == 0) {
             // _this.tableDatas = response.data.records;
            _this.tableDatas2 = JSON.parse(JSON.stringify(response.data.records)); 
            // _this.total = response.data.total;
            _this.total = JSON.parse(JSON.stringify(response.data.total));
            // _this.totalPage = response.data.pages;
            _this.totalPage = JSON.parse(JSON.stringify(response.data.pages));
          }
        })
        .catch((error) => {
          // catch 指请求出错的处理
          console.log(error);
        });
    },
    //溯源
    selectentErpriseBookTraceability(sourceTracing) {
      var _this = this;
      _this.$axios
        .post(_this.port + "tEnterpriseInfo/selectentErpriseBookTraceability", {
          sourceTracing: sourceTracing,
        })
        .then((response) => {
          if (response.code == 0) {
            _this.sc = null;
            _this.zc = null;
            _this.zy = null;
            _this.cz = null;
            _this.tattachment = response.data.tattachment;
            response.data.list.forEach((element) => {
              if (element.type == "sc") {
                _this.sc = element;
              } else if (element.type == "zc") {
                _this.zc = element;
              } else if (element.type == "zy") {
                _this.zy = element;
              } else if (element.type == "cz") {
                _this.cz = element;
              }
            });
          }
        })
        .catch((error) => {
          // catch 指请求出错的处理
          console.log(error);
        });
    },
    handleSizeChange(val) {
      // 改变每页显示的条数
      this.pageSize = val;
      // 注意：在改变每页显示的条数时，要将页码显示到第一页
      this.cur_page = 1;
      this.init(this.activeName);
    },
    handleCurrentChange(val) {
      this.cur_page = val;
      this.init(this.activeName);
    },
    backPage() {
      this.$router.push({ name: "managermentEnterpriseMonitor" });
    },
    details(item, type) {
      this.dialogVisible = true;
      this.selectentErpriseBookTraceability(item.sourceTracing);
      this.tableData = item;
      if (type == 1) {
        this.tableData.time = item.productionTime;
      } else if (type == 2) {
        this.tableData.time = item.storageTime;
      } else if (type == 3) {
        this.tableData.time = item.transferTime;
      } else if (type == 4) {
        this.tableData.time = item.acceptTime;
      }
      this.tableData.type = type;
      console.log(this.tableData);
    },
  },
};
</script>

<style scoped>
</style>
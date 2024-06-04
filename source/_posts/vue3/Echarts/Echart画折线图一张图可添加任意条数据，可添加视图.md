---
title: '2.Echart画任意条数折线图，可添加视图'
date: '2024-5-10 9:57:00'
description: 'Echart画任意条数折线图，可添加视图'
cover: "./image/haibiantian.jpg"
published: false
categories:               
    - Echart

---

# 

```bash

<template>

  <div class="tc-container">

    <el-form

        :model="queryParams"

        ref="queryRef"

        :inline="true"

        class="content-header-form"

        @submit.native.prevent

    >

      <el-form-item label="" prop="" class="search">

        <tableselect


            ref="tableselectRef"

            :parentoptions="searchList"

            @handleSearch="handleQuery"

            @resetQuery="resetQuery"

            :dateTime="true"

        ></tableselect>

      </el-form-item>

      <el-form-item class="button-item">

        <el-button

            type="primary"

            icon="Download"

            @click="handleDownload"

        >下载

        </el-button>

      </el-form-item>

      <el-form-item v-if="!chartview" class="button-item">

        <el-button

            type="primary"

            @click="handlechart"

        ><el-icon><TrendCharts /></el-icon>视图

        </el-button>

      </el-form-item>

      <el-form-item v-if="chartview" class="button-item">

        <el-button

            type="primary"

            @click="handletable"

        >列表

        </el-button>

      </el-form-item>

      <el-form-item v-if="chartview" class="button-item">

        <el-button

            type="primary"

            @click="chartadd"

        >添加视图

        </el-button>

      </el-form-item>

    </el-form>

    <el-table v-if="!chartview" v-loading="loading"

              :data="tableData">

      <el-table-column

          label="参数名称"

          align="center"

          prop="name"

          :show-overflow-tooltip="true"

      />

      <el-table-column

          label="参数代号"

          align="center"

          prop="code"

          :show-overflow-tooltip="true"

      />

      <el-table-column

          label="设备标识"

          align="center"

          prop="stationSymbol"

          :show-overflow-tooltip="true"

      />

      <el-table-column

          label="告警级别"

          align="center"

          prop="alarmLevel"

          :show-overflow-tooltip="true"

      />

      <el-table-column

          label="解析时间"

          align="center"

          prop="createTime"

          :show-overflow-tooltip="true"

      />

      <el-table-column

          label="星上时间(UTC)"

          align="center"

          prop="satelliteTime"

          :show-overflow-tooltip="true"

      />

      <el-table-column

          label="数据采样时间"

          align="center"

          prop="stationTime"

          :show-overflow-tooltip="true"

      />

      <el-table-column

          label="源码"

          align="center"

          prop="aryValue"

          :show-overflow-tooltip="true"

      >


        <template #header>

          <div class="slotBox">

            <span>源码</span>

            <el-select

                v-model="search"

                class="m-2"

                placeholder="Select"

                size="small"

                @change="selectVal"

                style="width: 80px; margin-left: 0.1rem"

            >

              <el-option

                  v-for="item in scaleOptions"

                  :key="item.value"

                  :label="item.label"

                  :value="item.value"

              />

            </el-select>

          </div>

        </template>

      </el-table-column>

      <el-table-column

          label="解析值"

          align="center"

          prop="value"

          :show-overflow-tooltip="true"

      />

    </el-table>

      <div v-if="chartview" v-for="(item,index) in chartList" :key="index+1" class="chart-container" style="width: 100%; height: 100%">

        <div class="right_chart" :id="'lineRef'+item.chartid" style="width: 100%;height: 100%"></div>

      <!--      <div class="right_chart" id="chartTwo" style="width: 50%;height:300px"></div>-->

      <!--      <div class="selected-one" style="width: 50%;height:50px">-->

      <!--        <el-select-->

      <!--            v-model="paramId"-->

      <!--            placeholder="请选择参数"-->

      <!--            @change="changeparam"-->

      <!--        >-->

      <!--          <el-option value>请选择</el-option>-->

      <!--          <el-option key="1" label="是" value="1"/>-->

      <!--          <el-option key="0" label="否" value="0"/>-->

      <!--&lt;!&ndash;          <el-option&ndash;&gt;-->

      <!--&lt;!&ndash;              v-for="item in enterpriseList"&ndash;&gt;-->

      <!--&lt;!&ndash;              :key="'enterprise' + item.compId"&ndash;&gt;-->

      <!--&lt;!&ndash;              :label="item.companyName"&ndash;&gt;-->

      <!--&lt;!&ndash;              :value="item.compId"&ndash;&gt;-->

      <!--&lt;!&ndash;          ></el-option>&ndash;&gt;-->

      <!--        </el-select>-->

      <!--      </div>-->

      <!--      <div class="selected-one" style="width: 50%;height:50px">-->

      <!--        <el-select-->

      <!--            v-model="paramId"-->

      <!--            placeholder="请选择企业空间"-->

      <!--            @change="changeparam"-->

      <!--        >-->

      <!--          <el-option value>请选择</el-option>-->

      <!--          <el-option key="1" label="是" value="1"/>-->

      <!--          <el-option key="0" label="否" value="0"/>-->

      <!--          &lt;!&ndash;          <el-option&ndash;&gt;-->

      <!--          &lt;!&ndash;              v-for="item in enterpriseList"&ndash;&gt;-->

      <!--          &lt;!&ndash;              :key="'enterprise' + item.compId"&ndash;&gt;-->

      <!--          &lt;!&ndash;              :label="item.companyName"&ndash;&gt;-->

      <!--          &lt;!&ndash;              :value="item.compId"&ndash;&gt;-->

      <!--          &lt;!&ndash;          ></el-option>&ndash;&gt;-->

      <!--        </el-select>-->

      <!--      </div>-->

      <!--      <div class="right_chart" id="chartThree" style="width: 50%;height:300px"></div>-->

      <!--      <div class="right_chart" id="chartFour" style="width: 50%;height:300px"></div>-->

      <!--      <div class="selected-one" style="width: 50%;height:50px">-->

      <!--        <el-select-->

      <!--            v-model="paramId"-->

      <!--            placeholder="请选择企业空间"-->

      <!--            @change="changeparam"-->

      <!--        >-->

      <!--          <el-option value>请选择</el-option>-->

      <!--          <el-option key="1" label="是" value="1"/>-->

      <!--          <el-option key="0" label="否" value="0"/>-->

      <!--          &lt;!&ndash;          <el-option&ndash;&gt;-->

      <!--          &lt;!&ndash;              v-for="item in enterpriseList"&ndash;&gt;-->

      <!--          &lt;!&ndash;              :key="'enterprise' + item.compId"&ndash;&gt;-->

      <!--          &lt;!&ndash;              :label="item.companyName"&ndash;&gt;-->

      <!--          &lt;!&ndash;              :value="item.compId"&ndash;&gt;-->

      <!--          &lt;!&ndash;          ></el-option>&ndash;&gt;-->

      <!--        </el-select>-->

      <!--      </div>-->

      <!--      <div class="selected-one" style="width: 50%;height:50px">-->

      <!--        <el-select-->

      <!--          v-model="paramId"-->

      <!--          placeholder="请选择企业空间"-->

      <!--          @change="changeparam"-->

      <!--      >-->

      <!--        <el-option value>请选择</el-option>-->

      <!--        <el-option key="1" label="是" value="1"/>-->

      <!--        <el-option key="0" label="否" value="0"/>-->

      <!--        &lt;!&ndash;          <el-option&ndash;&gt;-->

      <!--        &lt;!&ndash;              v-for="item in enterpriseList"&ndash;&gt;-->

      <!--        &lt;!&ndash;              :key="'enterprise' + item.compId"&ndash;&gt;-->

      <!--        &lt;!&ndash;              :label="item.companyName"&ndash;&gt;-->

      <!--        &lt;!&ndash;              :value="item.compId"&ndash;&gt;-->

      <!--        &lt;!&ndash;          ></el-option>&ndash;&gt;-->

      <!--      </el-select>-->

      <!--      </div>-->

    </div>

    <pagination

        v-show="total > 0 && !chartview"

        :total="total"

        v-model:page="queryParams.pageNum"

        v-model:limit="queryParams.pageSize"

        @pagination="getTrList"

    />

    <copyrightbottom v-if="!chartview"/>

  </div>

</template>

<script setup>

import Sortable from "sortablejs";

import * as API from "@/api/telemetering/tm_api.js";

import {nextTick, onMounted, reactive, ref, watch} from "vue";

import qs from "qs";

import usePermissionStore from "@/store/modules/permission";

import tableselect from "@/components/system/table-select.vue";

import {getStationList} from "@/api/telestation/api";

import Copyrightbottom from "@/components/copyrightbottom/index.vue";

import * as echarts from "echarts";

import Pagination from "@/components/Pagination/index.vue";

import edit from "@/views/satellite/telemetering/TelemetryAnalysis/edit.vue";

import {cloneDeepAndFormat} from "@/packages/utils";

import {set} from "xe-utils";

const chartview = ref(false)

const router = useRouter();

const total = ref(0);

const routeParams = ref([])

const optionOne = ref({

  title: {

    text: '遥测结果'

  },

  legend: {

    data:[]

  },

  tooltip: {

    trigger: 'axis',

  },

  xAxis: {

    type: 'category' ,

    boundaryGap: false,

  },

  yAxis: {

    type: 'category'

  },

  dataZoom: {

    show: true,

    start: 0,

    end: 100

  },

  grid: { top: '15%' },

  series: [

    {

      name: '',

      type: 'line',

      // smooth: true,

      // seriesLayoutBy: 'row',

      // connectNulls:true,

      // emphasis: { focus: 'series' },

      data: []

    },

    {

      name: '',

      type: 'line',

      // smooth: true,

      // seriesLayoutBy: 'row',

      // connectNulls:true,

      // emphasis: { focus: 'series' },

      data: []

    },

    {

      name: '',

      type: 'line',

      // smooth: true,

      // seriesLayoutBy: 'row',

      // connectNulls:true,

      // emphasis: { focus: 'series' },

      data: []

    },

    {

      name: '',

      type: 'line',

      // smooth: true,

      // seriesLayoutBy: 'row',

      // connectNulls:true,

      // emphasis: { focus: 'series' },

      data: []

    }

  ]

})

const arr1 = ref({

  title: {

    text: '遥测结果'

  },

  legend: {

    data:[]

  },

  tooltip: {

    trigger: 'axis',

  },

  xAxis: {

    type: 'category' ,

    boundaryGap: false,

  },

  yAxis: {

    type: 'category'

  },

  dataZoom: {

    show: true,

    start: 0,

    end: 100

  },

  grid: { top: '15%' },

  series: [

    {

      name: '',

      type: 'line',

      // smooth: true,

      // seriesLayoutBy: 'row',

      // connectNulls:true,

      // emphasis: { focus: 'series' },

      data: []

    },

  ]

})

// let mychartOne = {}


const permissionStore = usePermissionStore();

const {proxy} = getCurrentInstance();

const search = ref("2");

const tabNum = ref(0);

const scaleOptions = [

  {

    value: "2",

    label: "二进制",

  },

  {

    value: "10",

    label: "十进制",

  },

  {

    value: "16",

    label: "十六进制",

  },

];


const paramId = ref('')

const loading = ref(false);

const tableData = ref([])

const data = reactive({

  queryParams: {

    code: undefined,

    date: undefined,

    startTime: undefined,

    endTime: undefined,

    pageNum: 1,

    pageSize: 10,

  },

  searchList: [

    {

      id: "1",

      value: "code",

      label: "参数代号",

      type: "select",

      options: [],

    },

    {

      id: "2",

      value: "name",

      label: "参数名称",

      type: "input",

    },

    {

      id: "3",

      value: "date",

      label: "起止时间",

      type: "datepicker",

    },

    {

      id: "4",

      value: "delay",

      label: "遥测状态",

      type: "select",

      options: [

        {

          label: "延迟遥测",

          value: "true",

        },

        {

          label: "实时遥测",

          value: "false",

        },

      ],

    },

    {

      id: "5",

      value: "stationSymbol",

      label: "设备",

      type: "select",

      options: [],

    },

    {

      id: "6",

      value: "alarmLevel",

      label: "告警级别",

      type: "select",

      options: [

        {

          value: '0',

          label: '未告警'

        },

        {

          value: '1',

          label: '一级告警'

        },

        {

          value: '2',

          label: '二级告警'

        },

        {

          value: '3',

          label: '三级告警'

        }

      ],

    }

  ],

});

let {queryParams, searchList,} = toRefs(data);

onMounted(() => {

  getParamList();

  getTrList();

  // initSortable();

  getStationListuser()

});


// 视图

function handlechart() {

  chartview.value = true

  console.log('1111',chartview.value)

  set(chartList,0,{chartid:'chartOne',arr:cloneDeepAndFormat(arr1.value)})

  getTrList()

}

const chartList = reactive([{

  chartid:'chartOne',

  arr:[]

}])

const lineList = ref([0])

const mychartlList = ref([])

//添加视图

function chartadd(){

  console.log('jiaijiajiai')

  chartnumber.value = chartnumber.value + 1

  console.log('chartnumber.value-1]',chartnumber.value)

  set(chartList,chartnumber.value-1,{chartid:Math.random(),arr:cloneDeepAndFormat(arr1.value)})

  console.log('chartList[chartnumber.value-1]',chartList)

  lineChartOne()

}

const chartnumber = ref(1);

let arr2 = {};

let arr3 = {};

let arr4 = {};

function lineChartOne() {

  setTimeout(function () {

    if (chartnumber.value == 1){

      // let mychartOne = {}

      if (lineList.value[chartnumber.value-1] == 1){

        mychartlList.value[0] = echarts.init(document.getElementById('lineRef'+'chartOne'))

      }

      console.log('数组一',chartList[chartnumber.value-1].arr)

      mychartlList.value[0].setOption(chartList[chartnumber.value-1].arr)

    }else {

      if (!lineList.value[chartnumber.value-1]){

        lineList.value[chartnumber.value-1]=0

        mychartlList.value[chartnumber.value-1] = echarts.init(document.getElementById('lineRef'+chartList[chartnumber.value-1].chartid))

        console.log('二初始化',mychartlList.value)

      }

      let abb=chartList[chartnumber.value-1].arr

      console.log('数组二',abb)

      mychartlList.value[chartnumber.value-1].setOption(abb,true)

    }

  },1000)

}

function lineChartTwo() {

  console.log('2222222')

  setTimeout(function () {

    let mychartTwo = echarts.init(document.getElementById('lineRef'+chartList[1]))

    mychartTwo.setOption(arr2.value)

  },1000)

}

function lineChartThree() {

  console.log('3333333333')

  setTimeout(function () {

    let mychartThree = echarts.init(document.getElementById('lineRef'+chartList[2]))

    mychartThree.setOption(arr3.value)

  },1000)

}

function lineChartFour() {

  console.log('4444444444444444444')

  setTimeout(function () {

    let mychartFour = echarts.init(document.getElementById('lineRef'+chartList[3]))

    mychartFour.setOption(arr4.value)

  },1000)

}


// 视图

function handletable() {

  chartview.value = false

  console.log('1111',chartview.value)

  queryParams.value.pageSize = 10

  getTrList()

}




/** 获取测站列表 */

function getStationListuser() {

  let params = {};

  params.companyId = permissionStore.perSpaceId;

  getStationList(params).then(res => {

    if (res.code == 200) {

      searchList.value[4].options = [];

      console.log('测站列表',res.rows)

      res.rows.forEach((item) => {

        searchList.value[4].options.push({

          value: item.sign,

          label: item.name,

        });

      });

    }

  });

}


/** 搜索按钮操作 */

function handleQuery(action) {

  queryParams.value = {

    code: undefined,

    date: undefined,

    startTime: undefined,

    endTime: undefined,

  }

  formatSearchParams(action);

  queryParams.value.pageNum = 1;

  queryParams.value.pageSize = 10;

  getTrList();

}


function handleDownload() {

  console.log(queryParams.value)

  let param = {

    satelliteId: permissionStore.spaceId,

    code: queryParams.value.code,

    startTime: queryParams.value.date ? queryParams.value.date[0] : undefined,

    endTime: queryParams.value.date ? queryParams.value.date[1] : undefined

  }

  if (param.startTime && param.endTime && param.code) {

    console.log(param)

    proxy.download("datastorage/tmResults/export", {

      ...param,

    }, `${param.code+ '遥测结果' + new Date().getTime()}.xlsx`);

  } else {

    proxy.$modal.msgError("请选择时间与参数代号");

  }

}


//进制切换

function selectVal(val) {

  tableData.value.forEach((element) => {

    element.aryValue = transitionVal(element.binValue, val);

  });

}


function transitionVal(binValue, ary) {

  let newVal = null;

  if (!isNaN(binValue)) {

    if (ary == "10") {

      newVal = Number.parseInt(binValue, 2);

    } else if (ary == "16") {

      newVal = Number.parseInt(binValue, 2).toString(16);

    } else if (ary == "2") {

      newVal = binValue;

    }

  }

  return newVal;

}


/** 格式化搜索框子组件传递过来的参数 */

function formatSearchParams(action) {

  if (action && action.length > 0) {

    action.forEach((item) => {

      let order = item.indexOf(":");

      let name = item.substring(0, order);

      let value = item.substring(order + 1, item.length);

      switch (name) {

        case "参数代号":

          queryParams.value.code = searchList.value[0].options.filter(

              (item) => item.label == value

          )[0].value;

          break;

        case "参数名称":

          queryParams.value.name = value;

          break;

        case "起止时间":

          queryParams.value.date = [

            value.substring(0, 19),

            value.substring(20),

          ];

          break;

        case "遥测状态":

          queryParams.value.delay = value;

          break;

        case "设备":

          queryParams.value.stationSymbol = value;

          break;

        case "告警级别":

          console.log(value)

          queryParams.value.alarmLevel = value;

          break;

        default:

          break;

      }

    });

  } else {

    queryParams.value = {};

    queryParams.value.pageNum = 1;

    queryParams.value.pageSize = 10;

  }

}


/** 重置按钮操作 */

function resetQuery() {

  proxy.resetForm("queryRef");

  handleQuery();

  proxy.$refs["tableselectRef"].clearTags();

}


/** 获取遥测参数列表 */

async function getParamList() {

  let param = {satelliteId: permissionStore.spaceId};

  let res = await API.listParam(qs.stringify(param, {arrayFormat: "repeat"}));

  let paramList = [];

  if (res?.code == 200) {

    if (res.rows.length != 0) {

      res.rows.forEach((element) => {

        paramList.push({

          label: element.code,

          value: element.code,

          name: element.name

        });

      });

      searchList.value[0].options = paramList;

      console.log('res.rows',paramList)

      tabNum.value++;

    }

  }

}


const line1 = ref(0)

const line2 = ref(0)

const line3 = ref(0)

const line4 = ref(0)

// 查询遥测结果列表

async function getTrList() {

  let param = {

        satelliteId: permissionStore.spaceId,

        code: queryParams.value.code,

        startTime: queryParams.value.date ? queryParams.value.date[0] : undefined,

        endTime: queryParams.value.date ? queryParams.value.date[1] : undefined,

        pageNum: queryParams.value.pageNum,

        pageSize: queryParams.value.pageSize,

        name: queryParams.value.name,

        delay: queryParams.value.delay,

        stationSymbol: queryParams.value.stationSymbol,

        alarmLevel: queryParams.value.alarmLevel

      },

      res = null,

      resData = null;

  loading.value = true;

  const arr = Object.values(queryParams.value).filter(Boolean)

  console.log('arr',arr)

  if(arr.length > 3 ){

    if(chartview.value){

      try {

        res = await API.tmResultsList(

            qs.stringify(param, {arrayFormat: "repeat"})

        );

      } catch (error) {

        loading.value = false;

      }

      loading.value = false;

      res.rows.forEach((element) => {

        element.aryValue = element.binValue;

      });

      resData = res.rows;

      total.value = res.total;

      tableData.value = resData;

      let arra = [],arrb = []

      if (tableData.value.length>0) {

        console.log('line',lineList.value[chartnumber.value-1])

        console.log('tableData.value',tableData.value)

        tableData.value.map(item =>{

          arra.push(item.satelliteTime)

          arrb.push(item.value)

        })

        console.log(arra,arrb)

        if(chartnumber.value == 1){

          console.log(('开始数据'))

          lineList.value[chartnumber.value-1] = lineList.value[chartnumber.value-1] + 1

          chartList[chartnumber.value-1].arr.series[lineList.value[chartnumber.value-1]-1]={

            name: '',

            type: 'line',

            data: []

          }

          for (let i=0;i<arra.length;i++){

            let arrdata=[arra[i],arrb[i]]

            chartList[chartnumber.value-1].arr.series[lineList.value[chartnumber.value-1]-1].data.push(arrdata)

            console.log('bianli',chartList)

          }

          console.log('chartList',chartList)

          chartList[chartnumber.value-1].arr.series[lineList.value[chartnumber.value-1]-1].name = tableData.value[0].name

          chartList[chartnumber.value-1].arr.legend.data.push(tableData.value[0].name)

          lineChartOne()

        }else{

          lineList.value[chartnumber.value-1] = lineList.value[chartnumber.value-1] + 1

          chartList[chartnumber.value-1].arr.series[lineList.value[chartnumber.value-1]-1]={

            name: '',

            type: 'line',

            data: []

          }

          for (let i=0;i<arra.length;i++){

            let arrdata=[arra[i],arrb[i]]

            chartList[chartnumber.value-1].arr.series[lineList.value[chartnumber.value-1]-1].data.push(arrdata)

          }

          chartList[chartnumber.value-1].arr.series[lineList.value[chartnumber.value-1]-1].name = tableData.value[0].name

          chartList[chartnumber.value-1].arr.legend.data.push(tableData.value[0].name)

          lineChartOne()

        }

      }

    }else {

      try {

        res = await API.tmResultsList(

            qs.stringify(param, {arrayFormat: "repeat"})

        );

      } catch (error) {

        loading.value = false;

      }

      loading.value = false;

      res.rows.forEach((element) => {

        element.aryValue = element.binValue;

      });

      resData = res.rows;

      total.value = res.total;

      tableData.value = resData;

      console.log('没画图的表格数据',tableData.value)

    }

  }else {

    proxy.$modal.msgWarning("请选择两条或以上查询条件")

    loading.value = false;

  }

}


// 分页获取表结构

function currentChange(val) {

  queryParams.value.pageNum = val.pageNum;

  queryParams.value.pageSize = val.pageSize;

  getTrList();

}


function initSortable() {

  let sortable;

  // 拖动配置

  const tbody = document.querySelector(".vxe-table--body tbody");

  const ops = {

    animation: 200,

    group: "id",

    handle: ".col--seq", // handle's class

    // 拖动结束

    onEnd: function (evt) {

      // 获取拖动后的排序，arr数组里的值是 data-id 的顺序

      let arr = sortable.toArray();

      console.log({evt, arr});

      const currRow = tableData.value.splice(evt.oldIndex, 1)[0];

      console.log(currRow);

      tableData.value.splice(evt.newIndex, 0, currRow);

      console.log(Array.from(tableData.value));

      //   拖动后获取newIdex

    },

  };

  //初始化

  sortable = Sortable.create(tbody, ops);

}

</script>

<style lang="scss" scoped>

.tc-container {

  width: 100%;

  height: 100%;


  .tc-container-table {

    height: calc(100% - 52px);

    width: 100%;


    .content-header-form {

      width: 90%;

      background: transparent;

    }


    .slotBox {

      display: inline-block;

    }

  }

  .chart-container{

    display: flex;

    flex-wrap: wrap;

    .selected-one{

      width: 50%;

      height: 50px;

      display: flex;

      justify-content: center;


    }

  }

}

</style>


```

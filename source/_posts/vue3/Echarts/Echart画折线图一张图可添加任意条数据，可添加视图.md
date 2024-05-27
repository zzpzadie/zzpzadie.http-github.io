[//]: # (---)

[//]: # (title: '2.Echart画任意条数折线图，可添加视图')

[//]: # (date: '2024-5-10 9:57:00')

[//]: # (description: 'Echart画任意条数折线图，可添加视图')

[//]: # (cover: "./image/haibiantian.jpg")

[//]: # (categories:               )

[//]: # (    - Echart)

[//]: # (---)

[//]: # (# )

[//]: # (```bash)

[//]: # (<template>)

[//]: # (  <div class="tc-container">)

[//]: # (    <el-form)

[//]: # (        :model="queryParams")

[//]: # (        ref="queryRef")

[//]: # (        :inline="true")

[//]: # (        class="content-header-form")

[//]: # (        @submit.native.prevent)

[//]: # (    >)

[//]: # (      <el-form-item label="" prop="" class="search">)

[//]: # (        <tableselect)

[//]: # ()
[//]: # (            ref="tableselectRef")

[//]: # (            :parentoptions="searchList")

[//]: # (            @handleSearch="handleQuery")

[//]: # (            @resetQuery="resetQuery")

[//]: # (            :dateTime="true")

[//]: # (        ></tableselect>)

[//]: # (      </el-form-item>)

[//]: # (      <el-form-item class="button-item">)

[//]: # (        <el-button)

[//]: # (            type="primary")

[//]: # (            icon="Download")

[//]: # (            @click="handleDownload")

[//]: # (        >下载)

[//]: # (        </el-button>)

[//]: # (      </el-form-item>)

[//]: # (      <el-form-item v-if="!chartview" class="button-item">)

[//]: # (        <el-button)

[//]: # (            type="primary")

[//]: # (            @click="handlechart")

[//]: # (        ><el-icon><TrendCharts /></el-icon>视图)

[//]: # (        </el-button>)

[//]: # (      </el-form-item>)

[//]: # (      <el-form-item v-if="chartview" class="button-item">)

[//]: # (        <el-button)

[//]: # (            type="primary")

[//]: # (            @click="handletable")

[//]: # (        >列表)

[//]: # (        </el-button>)

[//]: # (      </el-form-item>)

[//]: # (      <el-form-item v-if="chartview" class="button-item">)

[//]: # (        <el-button)

[//]: # (            type="primary")

[//]: # (            @click="chartadd")

[//]: # (        >添加视图)

[//]: # (        </el-button>)

[//]: # (      </el-form-item>)

[//]: # (    </el-form>)

[//]: # (    <el-table v-if="!chartview" v-loading="loading")

[//]: # (              :data="tableData">)

[//]: # (      <el-table-column)

[//]: # (          label="参数名称")

[//]: # (          align="center")

[//]: # (          prop="name")

[//]: # (          :show-overflow-tooltip="true")

[//]: # (      />)

[//]: # (      <el-table-column)

[//]: # (          label="参数代号")

[//]: # (          align="center")

[//]: # (          prop="code")

[//]: # (          :show-overflow-tooltip="true")

[//]: # (      />)

[//]: # (      <el-table-column)

[//]: # (          label="设备标识")

[//]: # (          align="center")

[//]: # (          prop="stationSymbol")

[//]: # (          :show-overflow-tooltip="true")

[//]: # (      />)

[//]: # (      <el-table-column)

[//]: # (          label="告警级别")

[//]: # (          align="center")

[//]: # (          prop="alarmLevel")

[//]: # (          :show-overflow-tooltip="true")

[//]: # (      />)

[//]: # (      <el-table-column)

[//]: # (          label="解析时间")

[//]: # (          align="center")

[//]: # (          prop="createTime")

[//]: # (          :show-overflow-tooltip="true")

[//]: # (      />)

[//]: # (      <el-table-column)

[//]: # (          label="星上时间&#40;UTC&#41;")

[//]: # (          align="center")

[//]: # (          prop="satelliteTime")

[//]: # (          :show-overflow-tooltip="true")

[//]: # (      />)

[//]: # (      <el-table-column)

[//]: # (          label="数据采样时间")

[//]: # (          align="center")

[//]: # (          prop="stationTime")

[//]: # (          :show-overflow-tooltip="true")

[//]: # (      />)

[//]: # (      <el-table-column)

[//]: # (          label="源码")

[//]: # (          align="center")

[//]: # (          prop="aryValue")

[//]: # (          :show-overflow-tooltip="true")

[//]: # (      >)

[//]: # ()
[//]: # (        <template #header>)

[//]: # (          <div class="slotBox">)

[//]: # (            <span>源码</span>)

[//]: # (            <el-select)

[//]: # (                v-model="search")

[//]: # (                class="m-2")

[//]: # (                placeholder="Select")

[//]: # (                size="small")

[//]: # (                @change="selectVal")

[//]: # (                style="width: 80px; margin-left: 0.1rem")

[//]: # (            >)

[//]: # (              <el-option)

[//]: # (                  v-for="item in scaleOptions")

[//]: # (                  :key="item.value")

[//]: # (                  :label="item.label")

[//]: # (                  :value="item.value")

[//]: # (              />)

[//]: # (            </el-select>)

[//]: # (          </div>)

[//]: # (        </template>)

[//]: # (      </el-table-column>)

[//]: # (      <el-table-column)

[//]: # (          label="解析值")

[//]: # (          align="center")

[//]: # (          prop="value")

[//]: # (          :show-overflow-tooltip="true")

[//]: # (      />)

[//]: # (    </el-table>)

[//]: # (      <div v-if="chartview" v-for="&#40;item,index&#41; in chartList" :key="index+1" class="chart-container" style="width: 100%; height: 100%">)

[//]: # (        <div class="right_chart" :id="'lineRef'+item.chartid" style="width: 100%;height: 100%"></div>)

[//]: # (      <!--      <div class="right_chart" id="chartTwo" style="width: 50%;height:300px"></div>-->)

[//]: # (      <!--      <div class="selected-one" style="width: 50%;height:50px">-->)

[//]: # (      <!--        <el-select-->)

[//]: # (      <!--            v-model="paramId"-->)

[//]: # (      <!--            placeholder="请选择参数"-->)

[//]: # (      <!--            @change="changeparam"-->)

[//]: # (      <!--        >-->)

[//]: # (      <!--          <el-option value>请选择</el-option>-->)

[//]: # (      <!--          <el-option key="1" label="是" value="1"/>-->)

[//]: # (      <!--          <el-option key="0" label="否" value="0"/>-->)

[//]: # (      <!--&lt;!&ndash;          <el-option&ndash;&gt;-->)

[//]: # (      <!--&lt;!&ndash;              v-for="item in enterpriseList"&ndash;&gt;-->)

[//]: # (      <!--&lt;!&ndash;              :key="'enterprise' + item.compId"&ndash;&gt;-->)

[//]: # (      <!--&lt;!&ndash;              :label="item.companyName"&ndash;&gt;-->)

[//]: # (      <!--&lt;!&ndash;              :value="item.compId"&ndash;&gt;-->)

[//]: # (      <!--&lt;!&ndash;          ></el-option>&ndash;&gt;-->)

[//]: # (      <!--        </el-select>-->)

[//]: # (      <!--      </div>-->)

[//]: # (      <!--      <div class="selected-one" style="width: 50%;height:50px">-->)

[//]: # (      <!--        <el-select-->)

[//]: # (      <!--            v-model="paramId"-->)

[//]: # (      <!--            placeholder="请选择企业空间"-->)

[//]: # (      <!--            @change="changeparam"-->)

[//]: # (      <!--        >-->)

[//]: # (      <!--          <el-option value>请选择</el-option>-->)

[//]: # (      <!--          <el-option key="1" label="是" value="1"/>-->)

[//]: # (      <!--          <el-option key="0" label="否" value="0"/>-->)

[//]: # (      <!--          &lt;!&ndash;          <el-option&ndash;&gt;-->)

[//]: # (      <!--          &lt;!&ndash;              v-for="item in enterpriseList"&ndash;&gt;-->)

[//]: # (      <!--          &lt;!&ndash;              :key="'enterprise' + item.compId"&ndash;&gt;-->)

[//]: # (      <!--          &lt;!&ndash;              :label="item.companyName"&ndash;&gt;-->)

[//]: # (      <!--          &lt;!&ndash;              :value="item.compId"&ndash;&gt;-->)

[//]: # (      <!--          &lt;!&ndash;          ></el-option>&ndash;&gt;-->)

[//]: # (      <!--        </el-select>-->)

[//]: # (      <!--      </div>-->)

[//]: # (      <!--      <div class="right_chart" id="chartThree" style="width: 50%;height:300px"></div>-->)

[//]: # (      <!--      <div class="right_chart" id="chartFour" style="width: 50%;height:300px"></div>-->)

[//]: # (      <!--      <div class="selected-one" style="width: 50%;height:50px">-->)

[//]: # (      <!--        <el-select-->)

[//]: # (      <!--            v-model="paramId"-->)

[//]: # (      <!--            placeholder="请选择企业空间"-->)

[//]: # (      <!--            @change="changeparam"-->)

[//]: # (      <!--        >-->)

[//]: # (      <!--          <el-option value>请选择</el-option>-->)

[//]: # (      <!--          <el-option key="1" label="是" value="1"/>-->)

[//]: # (      <!--          <el-option key="0" label="否" value="0"/>-->)

[//]: # (      <!--          &lt;!&ndash;          <el-option&ndash;&gt;-->)

[//]: # (      <!--          &lt;!&ndash;              v-for="item in enterpriseList"&ndash;&gt;-->)

[//]: # (      <!--          &lt;!&ndash;              :key="'enterprise' + item.compId"&ndash;&gt;-->)

[//]: # (      <!--          &lt;!&ndash;              :label="item.companyName"&ndash;&gt;-->)

[//]: # (      <!--          &lt;!&ndash;              :value="item.compId"&ndash;&gt;-->)

[//]: # (      <!--          &lt;!&ndash;          ></el-option>&ndash;&gt;-->)

[//]: # (      <!--        </el-select>-->)

[//]: # (      <!--      </div>-->)

[//]: # (      <!--      <div class="selected-one" style="width: 50%;height:50px">-->)

[//]: # (      <!--        <el-select-->)

[//]: # (      <!--          v-model="paramId"-->)

[//]: # (      <!--          placeholder="请选择企业空间"-->)

[//]: # (      <!--          @change="changeparam"-->)

[//]: # (      <!--      >-->)

[//]: # (      <!--        <el-option value>请选择</el-option>-->)

[//]: # (      <!--        <el-option key="1" label="是" value="1"/>-->)

[//]: # (      <!--        <el-option key="0" label="否" value="0"/>-->)

[//]: # (      <!--        &lt;!&ndash;          <el-option&ndash;&gt;-->)

[//]: # (      <!--        &lt;!&ndash;              v-for="item in enterpriseList"&ndash;&gt;-->)

[//]: # (      <!--        &lt;!&ndash;              :key="'enterprise' + item.compId"&ndash;&gt;-->)

[//]: # (      <!--        &lt;!&ndash;              :label="item.companyName"&ndash;&gt;-->)

[//]: # (      <!--        &lt;!&ndash;              :value="item.compId"&ndash;&gt;-->)

[//]: # (      <!--        &lt;!&ndash;          ></el-option>&ndash;&gt;-->)

[//]: # (      <!--      </el-select>-->)

[//]: # (      <!--      </div>-->)

[//]: # (    </div>)

[//]: # (    <pagination)

[//]: # (        v-show="total > 0 && !chartview")

[//]: # (        :total="total")

[//]: # (        v-model:page="queryParams.pageNum")

[//]: # (        v-model:limit="queryParams.pageSize")

[//]: # (        @pagination="getTrList")

[//]: # (    />)

[//]: # (    <copyrightbottom v-if="!chartview"/>)

[//]: # (  </div>)

[//]: # (</template>)

[//]: # (<script setup>)

[//]: # (import Sortable from "sortablejs";)

[//]: # (import * as API from "@/api/telemetering/tm_api.js";)

[//]: # (import {nextTick, onMounted, reactive, ref, watch} from "vue";)

[//]: # (import qs from "qs";)

[//]: # (import usePermissionStore from "@/store/modules/permission";)

[//]: # (import tableselect from "@/components/system/table-select.vue";)

[//]: # (import {getStationList} from "@/api/telestation/api";)

[//]: # (import Copyrightbottom from "@/components/copyrightbottom/index.vue";)

[//]: # (import * as echarts from "echarts";)

[//]: # (import Pagination from "@/components/Pagination/index.vue";)

[//]: # (import edit from "@/views/satellite/telemetering/TelemetryAnalysis/edit.vue";)

[//]: # (import {cloneDeepAndFormat} from "@/packages/utils";)

[//]: # (import {set} from "xe-utils";)

[//]: # (const chartview = ref&#40;false&#41;)

[//]: # (const router = useRouter&#40;&#41;;)

[//]: # (const total = ref&#40;0&#41;;)

[//]: # (const routeParams = ref&#40;[]&#41;)

[//]: # (const optionOne = ref&#40;{)

[//]: # (  title: {)

[//]: # (    text: '遥测结果')

[//]: # (  },)

[//]: # (  legend: {)

[//]: # (    data:[])

[//]: # (  },)

[//]: # (  tooltip: {)

[//]: # (    trigger: 'axis',)

[//]: # (  },)

[//]: # (  xAxis: {)

[//]: # (    type: 'category' ,)

[//]: # (    boundaryGap: false,)

[//]: # (  },)

[//]: # (  yAxis: {)

[//]: # (    type: 'category')

[//]: # (  },)

[//]: # (  dataZoom: {)

[//]: # (    show: true,)

[//]: # (    start: 0,)

[//]: # (    end: 100)

[//]: # (  },)

[//]: # (  grid: { top: '15%' },)

[//]: # (  series: [)

[//]: # (    {)

[//]: # (      name: '',)

[//]: # (      type: 'line',)

[//]: # (      // smooth: true,)

[//]: # (      // seriesLayoutBy: 'row',)

[//]: # (      // connectNulls:true,)

[//]: # (      // emphasis: { focus: 'series' },)

[//]: # (      data: [])

[//]: # (    },)

[//]: # (    {)

[//]: # (      name: '',)

[//]: # (      type: 'line',)

[//]: # (      // smooth: true,)

[//]: # (      // seriesLayoutBy: 'row',)

[//]: # (      // connectNulls:true,)

[//]: # (      // emphasis: { focus: 'series' },)

[//]: # (      data: [])

[//]: # (    },)

[//]: # (    {)

[//]: # (      name: '',)

[//]: # (      type: 'line',)

[//]: # (      // smooth: true,)

[//]: # (      // seriesLayoutBy: 'row',)

[//]: # (      // connectNulls:true,)

[//]: # (      // emphasis: { focus: 'series' },)

[//]: # (      data: [])

[//]: # (    },)

[//]: # (    {)

[//]: # (      name: '',)

[//]: # (      type: 'line',)

[//]: # (      // smooth: true,)

[//]: # (      // seriesLayoutBy: 'row',)

[//]: # (      // connectNulls:true,)

[//]: # (      // emphasis: { focus: 'series' },)

[//]: # (      data: [])

[//]: # (    })

[//]: # (  ])

[//]: # (}&#41;)

[//]: # (const arr1 = ref&#40;{)

[//]: # (  title: {)

[//]: # (    text: '遥测结果')

[//]: # (  },)

[//]: # (  legend: {)

[//]: # (    data:[])

[//]: # (  },)

[//]: # (  tooltip: {)

[//]: # (    trigger: 'axis',)

[//]: # (  },)

[//]: # (  xAxis: {)

[//]: # (    type: 'category' ,)

[//]: # (    boundaryGap: false,)

[//]: # (  },)

[//]: # (  yAxis: {)

[//]: # (    type: 'category')

[//]: # (  },)

[//]: # (  dataZoom: {)

[//]: # (    show: true,)

[//]: # (    start: 0,)

[//]: # (    end: 100)

[//]: # (  },)

[//]: # (  grid: { top: '15%' },)

[//]: # (  series: [)

[//]: # (    {)

[//]: # (      name: '',)

[//]: # (      type: 'line',)

[//]: # (      // smooth: true,)

[//]: # (      // seriesLayoutBy: 'row',)

[//]: # (      // connectNulls:true,)

[//]: # (      // emphasis: { focus: 'series' },)

[//]: # (      data: [])

[//]: # (    },)

[//]: # (  ])

[//]: # (}&#41;)

[//]: # (// let mychartOne = {})

[//]: # ()
[//]: # (const permissionStore = usePermissionStore&#40;&#41;;)

[//]: # (const {proxy} = getCurrentInstance&#40;&#41;;)

[//]: # (const search = ref&#40;"2"&#41;;)

[//]: # (const tabNum = ref&#40;0&#41;;)

[//]: # (const scaleOptions = [)

[//]: # (  {)

[//]: # (    value: "2",)

[//]: # (    label: "二进制",)

[//]: # (  },)

[//]: # (  {)

[//]: # (    value: "10",)

[//]: # (    label: "十进制",)

[//]: # (  },)

[//]: # (  {)

[//]: # (    value: "16",)

[//]: # (    label: "十六进制",)

[//]: # (  },)

[//]: # (];)

[//]: # ()
[//]: # (const paramId = ref&#40;''&#41;)

[//]: # (const loading = ref&#40;false&#41;;)

[//]: # (const tableData = ref&#40;[]&#41;)

[//]: # (const data = reactive&#40;{)

[//]: # (  queryParams: {)

[//]: # (    code: undefined,)

[//]: # (    date: undefined,)

[//]: # (    startTime: undefined,)

[//]: # (    endTime: undefined,)

[//]: # (    pageNum: 1,)

[//]: # (    pageSize: 10,)

[//]: # (  },)

[//]: # (  searchList: [)

[//]: # (    {)

[//]: # (      id: "1",)

[//]: # (      value: "code",)

[//]: # (      label: "参数代号",)

[//]: # (      type: "select",)

[//]: # (      options: [],)

[//]: # (    },)

[//]: # (    {)

[//]: # (      id: "2",)

[//]: # (      value: "name",)

[//]: # (      label: "参数名称",)

[//]: # (      type: "input",)

[//]: # (    },)

[//]: # (    {)

[//]: # (      id: "3",)

[//]: # (      value: "date",)

[//]: # (      label: "起止时间",)

[//]: # (      type: "datepicker",)

[//]: # (    },)

[//]: # (    {)

[//]: # (      id: "4",)

[//]: # (      value: "delay",)

[//]: # (      label: "遥测状态",)

[//]: # (      type: "select",)

[//]: # (      options: [)

[//]: # (        {)

[//]: # (          label: "延迟遥测",)

[//]: # (          value: "true",)

[//]: # (        },)

[//]: # (        {)

[//]: # (          label: "实时遥测",)

[//]: # (          value: "false",)

[//]: # (        },)

[//]: # (      ],)

[//]: # (    },)

[//]: # (    {)

[//]: # (      id: "5",)

[//]: # (      value: "stationSymbol",)

[//]: # (      label: "设备",)

[//]: # (      type: "select",)

[//]: # (      options: [],)

[//]: # (    },)

[//]: # (    {)

[//]: # (      id: "6",)

[//]: # (      value: "alarmLevel",)

[//]: # (      label: "告警级别",)

[//]: # (      type: "select",)

[//]: # (      options: [)

[//]: # (        {)

[//]: # (          value: '0',)

[//]: # (          label: '未告警')

[//]: # (        },)

[//]: # (        {)

[//]: # (          value: '1',)

[//]: # (          label: '一级告警')

[//]: # (        },)

[//]: # (        {)

[//]: # (          value: '2',)

[//]: # (          label: '二级告警')

[//]: # (        },)

[//]: # (        {)

[//]: # (          value: '3',)

[//]: # (          label: '三级告警')

[//]: # (        })

[//]: # (      ],)

[//]: # (    })

[//]: # (  ],)

[//]: # (}&#41;;)

[//]: # (let {queryParams, searchList,} = toRefs&#40;data&#41;;)

[//]: # (onMounted&#40;&#40;&#41; => {)

[//]: # (  getParamList&#40;&#41;;)

[//]: # (  getTrList&#40;&#41;;)

[//]: # (  // initSortable&#40;&#41;;)

[//]: # (  getStationListuser&#40;&#41;)

[//]: # (}&#41;;)

[//]: # ()
[//]: # (// 视图)

[//]: # (function handlechart&#40;&#41; {)

[//]: # (  chartview.value = true)

[//]: # (  console.log&#40;'1111',chartview.value&#41;)

[//]: # (  set&#40;chartList,0,{chartid:'chartOne',arr:cloneDeepAndFormat&#40;arr1.value&#41;}&#41;)

[//]: # (  getTrList&#40;&#41;)

[//]: # (})

[//]: # (const chartList = reactive&#40;[{)

[//]: # (  chartid:'chartOne',)

[//]: # (  arr:[])

[//]: # (}]&#41;)

[//]: # (const lineList = ref&#40;[0]&#41;)

[//]: # (const mychartlList = ref&#40;[]&#41;)

[//]: # (//添加视图)

[//]: # (function chartadd&#40;&#41;{)

[//]: # (  console.log&#40;'jiaijiajiai'&#41;)

[//]: # (  chartnumber.value = chartnumber.value + 1)

[//]: # (  console.log&#40;'chartnumber.value-1]',chartnumber.value&#41;)

[//]: # (  set&#40;chartList,chartnumber.value-1,{chartid:Math.random&#40;&#41;,arr:cloneDeepAndFormat&#40;arr1.value&#41;}&#41;)

[//]: # (  console.log&#40;'chartList[chartnumber.value-1]',chartList&#41;)

[//]: # (  lineChartOne&#40;&#41;)

[//]: # (})

[//]: # (const chartnumber = ref&#40;1&#41;;)

[//]: # (let arr2 = {};)

[//]: # (let arr3 = {};)

[//]: # (let arr4 = {};)

[//]: # (function lineChartOne&#40;&#41; {)

[//]: # (  setTimeout&#40;function &#40;&#41; {)

[//]: # (    if &#40;chartnumber.value == 1&#41;{)

[//]: # (      // let mychartOne = {})

[//]: # (      if &#40;lineList.value[chartnumber.value-1] == 1&#41;{)

[//]: # (        mychartlList.value[0] = echarts.init&#40;document.getElementById&#40;'lineRef'+'chartOne'&#41;&#41;)

[//]: # (      })

[//]: # (      console.log&#40;'数组一',chartList[chartnumber.value-1].arr&#41;)

[//]: # (      mychartlList.value[0].setOption&#40;chartList[chartnumber.value-1].arr&#41;)

[//]: # (    }else {)

[//]: # (      if &#40;!lineList.value[chartnumber.value-1]&#41;{)

[//]: # (        lineList.value[chartnumber.value-1]=0)

[//]: # (        mychartlList.value[chartnumber.value-1] = echarts.init&#40;document.getElementById&#40;'lineRef'+chartList[chartnumber.value-1].chartid&#41;&#41;)

[//]: # (        console.log&#40;'二初始化',mychartlList.value&#41;)

[//]: # (      })

[//]: # (      let abb=chartList[chartnumber.value-1].arr)

[//]: # (      console.log&#40;'数组二',abb&#41;)

[//]: # (      mychartlList.value[chartnumber.value-1].setOption&#40;abb,true&#41;)

[//]: # (    })

[//]: # (  },1000&#41;)

[//]: # (})

[//]: # (function lineChartTwo&#40;&#41; {)

[//]: # (  console.log&#40;'2222222'&#41;)

[//]: # (  setTimeout&#40;function &#40;&#41; {)

[//]: # (    let mychartTwo = echarts.init&#40;document.getElementById&#40;'lineRef'+chartList[1]&#41;&#41;)

[//]: # (    mychartTwo.setOption&#40;arr2.value&#41;)

[//]: # (  },1000&#41;)

[//]: # (})

[//]: # (function lineChartThree&#40;&#41; {)

[//]: # (  console.log&#40;'3333333333'&#41;)

[//]: # (  setTimeout&#40;function &#40;&#41; {)

[//]: # (    let mychartThree = echarts.init&#40;document.getElementById&#40;'lineRef'+chartList[2]&#41;&#41;)

[//]: # (    mychartThree.setOption&#40;arr3.value&#41;)

[//]: # (  },1000&#41;)

[//]: # (})

[//]: # (function lineChartFour&#40;&#41; {)

[//]: # (  console.log&#40;'4444444444444444444'&#41;)

[//]: # (  setTimeout&#40;function &#40;&#41; {)

[//]: # (    let mychartFour = echarts.init&#40;document.getElementById&#40;'lineRef'+chartList[3]&#41;&#41;)

[//]: # (    mychartFour.setOption&#40;arr4.value&#41;)

[//]: # (  },1000&#41;)

[//]: # (})

[//]: # ()
[//]: # (// 视图)

[//]: # (function handletable&#40;&#41; {)

[//]: # (  chartview.value = false)

[//]: # (  console.log&#40;'1111',chartview.value&#41;)

[//]: # (  queryParams.value.pageSize = 10)

[//]: # (  getTrList&#40;&#41;)

[//]: # (})

[//]: # ()
[//]: # ()
[//]: # ()
[//]: # (/** 获取测站列表 */)

[//]: # (function getStationListuser&#40;&#41; {)

[//]: # (  let params = {};)

[//]: # (  params.companyId = permissionStore.perSpaceId;)

[//]: # (  getStationList&#40;params&#41;.then&#40;res => {)

[//]: # (    if &#40;res.code == 200&#41; {)

[//]: # (      searchList.value[4].options = [];)

[//]: # (      console.log&#40;'测站列表',res.rows&#41;)

[//]: # (      res.rows.forEach&#40;&#40;item&#41; => {)

[//]: # (        searchList.value[4].options.push&#40;{)

[//]: # (          value: item.sign,)

[//]: # (          label: item.name,)

[//]: # (        }&#41;;)

[//]: # (      }&#41;;)

[//]: # (    })

[//]: # (  }&#41;;)

[//]: # (})

[//]: # ()
[//]: # (/** 搜索按钮操作 */)

[//]: # (function handleQuery&#40;action&#41; {)

[//]: # (  queryParams.value = {)

[//]: # (    code: undefined,)

[//]: # (    date: undefined,)

[//]: # (    startTime: undefined,)

[//]: # (    endTime: undefined,)

[//]: # (  })

[//]: # (  formatSearchParams&#40;action&#41;;)

[//]: # (  queryParams.value.pageNum = 1;)

[//]: # (  queryParams.value.pageSize = 10;)

[//]: # (  getTrList&#40;&#41;;)

[//]: # (})

[//]: # ()
[//]: # (function handleDownload&#40;&#41; {)

[//]: # (  console.log&#40;queryParams.value&#41;)

[//]: # (  let param = {)

[//]: # (    satelliteId: permissionStore.spaceId,)

[//]: # (    code: queryParams.value.code,)

[//]: # (    startTime: queryParams.value.date ? queryParams.value.date[0] : undefined,)

[//]: # (    endTime: queryParams.value.date ? queryParams.value.date[1] : undefined)

[//]: # (  })

[//]: # (  if &#40;param.startTime && param.endTime && param.code&#41; {)

[//]: # (    console.log&#40;param&#41;)

[//]: # (    proxy.download&#40;"datastorage/tmResults/export", {)

[//]: # (      ...param,)

[//]: # (    }, `${param.code+ '遥测结果' + new Date&#40;&#41;.getTime&#40;&#41;}.xlsx`&#41;;)

[//]: # (  } else {)

[//]: # (    proxy.$modal.msgError&#40;"请选择时间与参数代号"&#41;;)

[//]: # (  })

[//]: # (})

[//]: # ()
[//]: # (//进制切换)

[//]: # (function selectVal&#40;val&#41; {)

[//]: # (  tableData.value.forEach&#40;&#40;element&#41; => {)

[//]: # (    element.aryValue = transitionVal&#40;element.binValue, val&#41;;)

[//]: # (  }&#41;;)

[//]: # (})

[//]: # ()
[//]: # (function transitionVal&#40;binValue, ary&#41; {)

[//]: # (  let newVal = null;)

[//]: # (  if &#40;!isNaN&#40;binValue&#41;&#41; {)

[//]: # (    if &#40;ary == "10"&#41; {)

[//]: # (      newVal = Number.parseInt&#40;binValue, 2&#41;;)

[//]: # (    } else if &#40;ary == "16"&#41; {)

[//]: # (      newVal = Number.parseInt&#40;binValue, 2&#41;.toString&#40;16&#41;;)

[//]: # (    } else if &#40;ary == "2"&#41; {)

[//]: # (      newVal = binValue;)

[//]: # (    })

[//]: # (  })

[//]: # (  return newVal;)

[//]: # (})

[//]: # ()
[//]: # (/** 格式化搜索框子组件传递过来的参数 */)

[//]: # (function formatSearchParams&#40;action&#41; {)

[//]: # (  if &#40;action && action.length > 0&#41; {)

[//]: # (    action.forEach&#40;&#40;item&#41; => {)

[//]: # (      let order = item.indexOf&#40;":"&#41;;)

[//]: # (      let name = item.substring&#40;0, order&#41;;)

[//]: # (      let value = item.substring&#40;order + 1, item.length&#41;;)

[//]: # (      switch &#40;name&#41; {)

[//]: # (        case "参数代号":)

[//]: # (          queryParams.value.code = searchList.value[0].options.filter&#40;)

[//]: # (              &#40;item&#41; => item.label == value)

[//]: # (          &#41;[0].value;)

[//]: # (          break;)

[//]: # (        case "参数名称":)

[//]: # (          queryParams.value.name = value;)

[//]: # (          break;)

[//]: # (        case "起止时间":)

[//]: # (          queryParams.value.date = [)

[//]: # (            value.substring&#40;0, 19&#41;,)

[//]: # (            value.substring&#40;20&#41;,)

[//]: # (          ];)

[//]: # (          break;)

[//]: # (        case "遥测状态":)

[//]: # (          queryParams.value.delay = value;)

[//]: # (          break;)

[//]: # (        case "设备":)

[//]: # (          queryParams.value.stationSymbol = value;)

[//]: # (          break;)

[//]: # (        case "告警级别":)

[//]: # (          console.log&#40;value&#41;)

[//]: # (          queryParams.value.alarmLevel = value;)

[//]: # (          break;)

[//]: # (        default:)

[//]: # (          break;)

[//]: # (      })

[//]: # (    }&#41;;)

[//]: # (  } else {)

[//]: # (    queryParams.value = {};)

[//]: # (    queryParams.value.pageNum = 1;)

[//]: # (    queryParams.value.pageSize = 10;)

[//]: # (  })

[//]: # (})

[//]: # ()
[//]: # (/** 重置按钮操作 */)

[//]: # (function resetQuery&#40;&#41; {)

[//]: # (  proxy.resetForm&#40;"queryRef"&#41;;)

[//]: # (  handleQuery&#40;&#41;;)

[//]: # (  proxy.$refs["tableselectRef"].clearTags&#40;&#41;;)

[//]: # (})

[//]: # ()
[//]: # (/** 获取遥测参数列表 */)

[//]: # (async function getParamList&#40;&#41; {)

[//]: # (  let param = {satelliteId: permissionStore.spaceId};)

[//]: # (  let res = await API.listParam&#40;qs.stringify&#40;param, {arrayFormat: "repeat"}&#41;&#41;;)

[//]: # (  let paramList = [];)

[//]: # (  if &#40;res?.code == 200&#41; {)

[//]: # (    if &#40;res.rows.length != 0&#41; {)

[//]: # (      res.rows.forEach&#40;&#40;element&#41; => {)

[//]: # (        paramList.push&#40;{)

[//]: # (          label: element.code,)

[//]: # (          value: element.code,)

[//]: # (          name: element.name)

[//]: # (        }&#41;;)

[//]: # (      }&#41;;)

[//]: # (      searchList.value[0].options = paramList;)

[//]: # (      console.log&#40;'res.rows',paramList&#41;)

[//]: # (      tabNum.value++;)

[//]: # (    })

[//]: # (  })

[//]: # (})

[//]: # ()
[//]: # (const line1 = ref&#40;0&#41;)

[//]: # (const line2 = ref&#40;0&#41;)

[//]: # (const line3 = ref&#40;0&#41;)

[//]: # (const line4 = ref&#40;0&#41;)

[//]: # (// 查询遥测结果列表)

[//]: # (async function getTrList&#40;&#41; {)

[//]: # (  let param = {)

[//]: # (        satelliteId: permissionStore.spaceId,)

[//]: # (        code: queryParams.value.code,)

[//]: # (        startTime: queryParams.value.date ? queryParams.value.date[0] : undefined,)

[//]: # (        endTime: queryParams.value.date ? queryParams.value.date[1] : undefined,)

[//]: # (        pageNum: queryParams.value.pageNum,)

[//]: # (        pageSize: queryParams.value.pageSize,)

[//]: # (        name: queryParams.value.name,)

[//]: # (        delay: queryParams.value.delay,)

[//]: # (        stationSymbol: queryParams.value.stationSymbol,)

[//]: # (        alarmLevel: queryParams.value.alarmLevel)

[//]: # (      },)

[//]: # (      res = null,)

[//]: # (      resData = null;)

[//]: # (  loading.value = true;)

[//]: # (  const arr = Object.values&#40;queryParams.value&#41;.filter&#40;Boolean&#41;)

[//]: # (  console.log&#40;'arr',arr&#41;)

[//]: # (  if&#40;arr.length > 3 &#41;{)

[//]: # (    if&#40;chartview.value&#41;{)

[//]: # (      try {)

[//]: # (        res = await API.tmResultsList&#40;)

[//]: # (            qs.stringify&#40;param, {arrayFormat: "repeat"}&#41;)

[//]: # (        &#41;;)

[//]: # (      } catch &#40;error&#41; {)

[//]: # (        loading.value = false;)

[//]: # (      })

[//]: # (      loading.value = false;)

[//]: # (      res.rows.forEach&#40;&#40;element&#41; => {)

[//]: # (        element.aryValue = element.binValue;)

[//]: # (      }&#41;;)

[//]: # (      resData = res.rows;)

[//]: # (      total.value = res.total;)

[//]: # (      tableData.value = resData;)

[//]: # (      let arra = [],arrb = [])

[//]: # (      if &#40;tableData.value.length>0&#41; {)

[//]: # (        console.log&#40;'line',lineList.value[chartnumber.value-1]&#41;)

[//]: # (        console.log&#40;'tableData.value',tableData.value&#41;)

[//]: # (        tableData.value.map&#40;item =>{)

[//]: # (          arra.push&#40;item.satelliteTime&#41;)

[//]: # (          arrb.push&#40;item.value&#41;)

[//]: # (        }&#41;)

[//]: # (        console.log&#40;arra,arrb&#41;)

[//]: # (        if&#40;chartnumber.value == 1&#41;{)

[//]: # (          console.log&#40;&#40;'开始数据'&#41;&#41;)

[//]: # (          lineList.value[chartnumber.value-1] = lineList.value[chartnumber.value-1] + 1)

[//]: # (          chartList[chartnumber.value-1].arr.series[lineList.value[chartnumber.value-1]-1]={)

[//]: # (            name: '',)

[//]: # (            type: 'line',)

[//]: # (            data: [])

[//]: # (          })

[//]: # (          for &#40;let i=0;i<arra.length;i++&#41;{)

[//]: # (            let arrdata=[arra[i],arrb[i]])

[//]: # (            chartList[chartnumber.value-1].arr.series[lineList.value[chartnumber.value-1]-1].data.push&#40;arrdata&#41;)

[//]: # (            console.log&#40;'bianli',chartList&#41;)

[//]: # (          })

[//]: # (          console.log&#40;'chartList',chartList&#41;)

[//]: # (          chartList[chartnumber.value-1].arr.series[lineList.value[chartnumber.value-1]-1].name = tableData.value[0].name)

[//]: # (          chartList[chartnumber.value-1].arr.legend.data.push&#40;tableData.value[0].name&#41;)

[//]: # (          lineChartOne&#40;&#41;)

[//]: # (        }else{)

[//]: # (          lineList.value[chartnumber.value-1] = lineList.value[chartnumber.value-1] + 1)

[//]: # (          chartList[chartnumber.value-1].arr.series[lineList.value[chartnumber.value-1]-1]={)

[//]: # (            name: '',)

[//]: # (            type: 'line',)

[//]: # (            data: [])

[//]: # (          })

[//]: # (          for &#40;let i=0;i<arra.length;i++&#41;{)

[//]: # (            let arrdata=[arra[i],arrb[i]])

[//]: # (            chartList[chartnumber.value-1].arr.series[lineList.value[chartnumber.value-1]-1].data.push&#40;arrdata&#41;)

[//]: # (          })

[//]: # (          chartList[chartnumber.value-1].arr.series[lineList.value[chartnumber.value-1]-1].name = tableData.value[0].name)

[//]: # (          chartList[chartnumber.value-1].arr.legend.data.push&#40;tableData.value[0].name&#41;)

[//]: # (          lineChartOne&#40;&#41;)

[//]: # (        })

[//]: # (      })

[//]: # (    }else {)

[//]: # (      try {)

[//]: # (        res = await API.tmResultsList&#40;)

[//]: # (            qs.stringify&#40;param, {arrayFormat: "repeat"}&#41;)

[//]: # (        &#41;;)

[//]: # (      } catch &#40;error&#41; {)

[//]: # (        loading.value = false;)

[//]: # (      })

[//]: # (      loading.value = false;)

[//]: # (      res.rows.forEach&#40;&#40;element&#41; => {)

[//]: # (        element.aryValue = element.binValue;)

[//]: # (      }&#41;;)

[//]: # (      resData = res.rows;)

[//]: # (      total.value = res.total;)

[//]: # (      tableData.value = resData;)

[//]: # (      console.log&#40;'没画图的表格数据',tableData.value&#41;)

[//]: # (    })

[//]: # (  }else {)

[//]: # (    proxy.$modal.msgWarning&#40;"请选择两条或以上查询条件"&#41;)

[//]: # (    loading.value = false;)

[//]: # (  })

[//]: # (})

[//]: # ()
[//]: # (// 分页获取表结构)

[//]: # (function currentChange&#40;val&#41; {)

[//]: # (  queryParams.value.pageNum = val.pageNum;)

[//]: # (  queryParams.value.pageSize = val.pageSize;)

[//]: # (  getTrList&#40;&#41;;)

[//]: # (})

[//]: # ()
[//]: # (function initSortable&#40;&#41; {)

[//]: # (  let sortable;)

[//]: # (  // 拖动配置)

[//]: # (  const tbody = document.querySelector&#40;".vxe-table--body tbody"&#41;;)

[//]: # (  const ops = {)

[//]: # (    animation: 200,)

[//]: # (    group: "id",)

[//]: # (    handle: ".col--seq", // handle's class)

[//]: # (    // 拖动结束)

[//]: # (    onEnd: function &#40;evt&#41; {)

[//]: # (      // 获取拖动后的排序，arr数组里的值是 data-id 的顺序)

[//]: # (      let arr = sortable.toArray&#40;&#41;;)

[//]: # (      console.log&#40;{evt, arr}&#41;;)

[//]: # (      const currRow = tableData.value.splice&#40;evt.oldIndex, 1&#41;[0];)

[//]: # (      console.log&#40;currRow&#41;;)

[//]: # (      tableData.value.splice&#40;evt.newIndex, 0, currRow&#41;;)

[//]: # (      console.log&#40;Array.from&#40;tableData.value&#41;&#41;;)

[//]: # (      //   拖动后获取newIdex)

[//]: # (    },)

[//]: # (  };)

[//]: # (  //初始化)

[//]: # (  sortable = Sortable.create&#40;tbody, ops&#41;;)

[//]: # (})

[//]: # (</script>)

[//]: # (<style lang="scss" scoped>)

[//]: # (.tc-container {)

[//]: # (  width: 100%;)

[//]: # (  height: 100%;)

[//]: # ()
[//]: # (  .tc-container-table {)

[//]: # (    height: calc&#40;100% - 52px&#41;;)

[//]: # (    width: 100%;)

[//]: # ()
[//]: # (    .content-header-form {)

[//]: # (      width: 90%;)

[//]: # (      background: transparent;)

[//]: # (    })

[//]: # ()
[//]: # (    .slotBox {)

[//]: # (      display: inline-block;)

[//]: # (    })

[//]: # (  })

[//]: # (  .chart-container{)

[//]: # (    display: flex;)

[//]: # (    flex-wrap: wrap;)

[//]: # (    .selected-one{)

[//]: # (      width: 50%;)

[//]: # (      height: 50px;)

[//]: # (      display: flex;)

[//]: # (      justify-content: center;)

[//]: # ()
[//]: # (    })

[//]: # (  })

[//]: # (})

[//]: # (</style>)

[//]: # ()
[//]: # (```)

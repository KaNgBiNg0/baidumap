<template>
  <div id="Map_search" class="width12" :class="isShowCompanyData ? 'dataShowing' : ''">
    <baidu-map
      class="bm-view"
      ak="gs9pxTGgbgUBhK9d6nmv8U6jnUyVx9Y4"
      :center="center"
      :zoom="zoom"
      @ready="initMap"
      :scroll-wheel-zoom="!isInDrawing"
      @zoomend="zoomEnd"
      @moveend="moveEnd"
      @mousemove="movedraw"
      :dragging="!isInDrawing"
      :double-click-zoom="!isInDrawing"
      :keyboard="!isInDrawing"
      :min-zoom=5
      :map-click="false"
    >
        <div class="control_k">
            <button @click="zoom++" class="add_k">
              <i class="el-icon-plus"></i>
            </button>
            <button @click="zoom--"><i class="el-icon-minus"></i></button>
        </div>
      
      <bm-local-search
        :keyword="searchkeyword"
        :auto-viewport="true"
        @searchcomplete="searchcomplete"
        :panel="false"
      ></bm-local-search>
      <bm-polyline
        :path="polyPointArray"
        stroke-color="blue"
        :stroke-opacity="0.5"
        :stroke-weight="2"
      />
      
        <my-overlay
          :position="{ lng: item.long, lat: item.lat}"
          v-for="(item, index) in markerArrs"
          :key="index"
          :companyData="item"
          :infoWindowOpen="infoWindowOpen"
        >
        </my-overlay>
    </baidu-map>
  
    <div id="bg"></div>

    <div id="mapTools" class="disable" >

      <div class="topBlank"></div>
    
      <div class="center">
      
        <div class="leftBlank"></div>

        <div class="content">
          
          <!-- 搜索框 -->
          <searchCompanyInput :formInline="formInline" :searchByStationName="searchByStationName"></searchCompanyInput>

          <!-- 过滤器 -->
          <searchKeyCon :addtags="addtags" :removetags="removetags" :drawing="drawing" :isInDrawing="isInDrawing" :nodrawering="nodrawering"></searchKeyCon>

          <!-- 候选单位列表 -->
          <companyListPanel :list="drawRESULT" :queryData="queryData"></companyListPanel>

          <!-- 数据结果列表 -->i
          <companyDataPanel :list="drawRESULT" :isShowCompanyData="isShowCompanyData" :closeCompanyDataPanel="closeCompanyDataPanel"></companyDataPanel>
        
        </div>

        <div class="rightBlank"></div>

      </div>
      
      <div class="bottomBlank"></div>

    </div>

  </div>
</template>

<script type="text/ecmascript-6">
import BaiduMap from 'vue-baidu-map/components/map/Map.vue'
import BmLabel from 'vue-baidu-map/components/overlays/Label.vue'
import BmMarker from 'vue-baidu-map/components/overlays/Marker.vue'
import BmPolyline  from 'vue-baidu-map/components/overlays/Polyline.vue'
import BmPolygon  from 'vue-baidu-map/components/overlays/Polygon.vue'
import BmControl  from 'vue-baidu-map/components/controls/Control.vue'
import BmLocalSearch from 'vue-baidu-map/components/search/LocalSearch.vue'
import {BmlMarkerClusterer} from 'vue-baidu-map'

import country from './json/country.json'
import province  from './json/province.json'
import region from './json/region.json'
import city from './json/city.json'
import street from './json/street.json'
import dot from './json/dot.json'

import searchCompanyInput from './searchCompanyInput'
import searchKeyCon from './searchKeyCon'
import showKeys from './showKeys'
import MyOverlay from './MyOverlay'

// 候选公司列表
import companyListPanel from './companyListPanel';
// 查询结果
import companyDataPanel from './companyDataPanel';



  export default {
    components: {
      BaiduMap,
      BmLabel,
      BmMarker,
      BmLocalSearch,
      BmPolyline,
      BmPolygon,
      BmlMarkerClusterer,
      BmControl,

      MyOverlay,
      searchCompanyInput,
      searchKeyCon,

      companyListPanel,
      companyDataPanel
    },
    data(){
      return{
        country,
        province,
        region,
        city,
        street,
        dot,
        markerArrs:[{
          text:'暂无数据',
          long: "116.305434",
          lat: "39.96549",
          count: "0"
        }],
        /* 界限 */
        active: false,
        searchRESULT:[],
        filterRESULT:[],
        drawRESULT:[],
        /* 界限 */
        mockKeyWord:'',
        formInline: {
          address: '',
          addressReasult: ''
        },
        labelShow:false,
        searchkeyword:'',
        //多边形
        polygon:null,
        ploygonOpacity:0.4,
        pointArray:[],
        //是否处于画圈状态下
        isInDrawing :false,
        //是否处于鼠标左键按下状态下
        isMouseDown : false,
        //存储画出折线点的数组
        polyPointArray : [],
        //上次操作画出的折线
        lastPolyLine : null,
        //画圈完成后生成的多边形
        polygonAfterDraw : [],
        /* 浮层的显示结果 */
        addressReasult : [],
        // 是否显示查询数据结果
        isShowCompanyData: false,
        /* 初始化 */
        map:null,
        center: {lng: 116.404, lat: 39.915},
        zoom: 13,
        flag:true,//是否是第一集
        adds:[], //浮层显示数组
        addindex:0,
        mock:false,
        /* tags */
        tagsShow:false,
        tags: [
          
        ],
        allTags:[
          {name:'备选项1'},
          {name:'备选项2'},
          {name:'备选项3'},
          {name:'备选项4'},
          {name:'备选项5'},
          {name:'备选项6'},
          {name:'备选项7'},
          {name:'备选项8'},
          {name:'备选项9'},
          {name:'备选项10'},
        ]
      }
    },
    watch: {
      tags(val,old){
        if(this.tags.length===0){
          this.tagsShow = false;
        }else{
          this.tagsShow = true;
        }
      }
    },
    methods: {
      /* 初始化地图 */
      initMap ({BMap, map}) {
          this.center.lng = 116.404;
          this.center.lat = 39.915;
          this.zoom = 13;
          if(map){this.map = map;}
      },
      /* tags */
      addtags(index){
        this.tags.push(this.allTags[index-1])
      },
      removetags(index){
        const allTags = this.allTags;
        const tags = this.tags.filter((value,i)=>{
          return value.name !== allTags[index-1].name
          })
        this.tags=tags;
      },
      /* 初始化画图 */
      drawing(){
        this.isInDrawing = true;
        if(this.zoom <= 15){
            alert("请放大进行");
            this.isInDrawing = false;
        }else{
          this.map.clearOverlays();
          this.markerArrs = [];
         /* 有问题 */
          this.map.setDefaultCursor('crosshair');
          this.map.addEventListener('mousedown',  (e)=> {
            //如果处于画圈状态下,清空上次画圈的数据结构,设置isMouseDown进入画圈鼠标按下状态
            if(this.isInDrawing){
              this.polyPointArray=[];
              this.isMouseDown = true;
              this.markerArrs = [];
              try {
                this.map.clearOverlays();
              } catch (error) {

              }
            }

          });
          this.map.addEventListener('mouseup',  (e)=> {
            //如果处于画圈状态下,清空上次画圈的数据结构,设置isMouseDown进入画圈鼠标按下状态
            if (this.isInDrawing && this.isMouseDown) {
            this.isMouseDown = false;
            var polyPointArray = this.polyPointArray;
            var polygon = new window.BMap.Polygon(polyPointArray, {
              strokeColor: '#46ACFF',
              strokeOpacity: 1,
              fillColor: '#46ACFF',
              fillOpacity: 0.3,
              enableClicking: false
            });
            this.map.addOverlay(polygon);
            //包含情况
            this.show(polygon);
            }
          });
        }
      },
      movedraw(e){
        if(this.isMouseDown && this.isInDrawing){
          this.polyPointArray.push(e.point);
        }
      },
      nodrawering(){
        this.isInDrawing = false;
        this.map.setDefaultCursor('default');
        this.map.clearOverlays();
        const markerArrs = this.zoomLevel();
        this.markerArrs = markerArrs;
        console.log(this.drawRESULT)
        this.drawRESULT = [];
        console.log(this.drawRESULT)
      },
      updatePolygonPath(e){
        this.polygon = e.target;
        if(this.polygon.getPath().length !==0){
        this.pointArray = this.polygon.getPath();}
      },
      show(polygon) {
          //得到多边形的点数组
          var pointArray = polygon.getPath();
          //获取多边形的外包矩形
          var bound = polygon.getBounds();
          //在多边形内的点的数组
          let  pointInPolygonArray = [];
          //计算每个点是否包含在该多边形内
          let keys = Object.keys(this.dot)
          for (var i = 0; i < this.dot.length; i++) {
            //该marker的坐标点
            let element  = this.dot[i];
            let {long,lat} = element;
            var marker = new BMap.Marker(new BMap.Point(long,lat)); // 创建点
	          let markerPoint = marker.getPosition();
            if (this.isPointInPolygon(markerPoint, bound, pointArray)) {
              pointInPolygonArray.push(this.dot[i])
            }
          }
          if(pointInPolygonArray.length!==0){
            this.markerArrs = pointInPolygonArray;
            this.drawRESULT = pointInPolygonArray;
          }
          
      },
      isPointInPolygon(point, bound, pointArray) {
        //首先判断该点是否在外包矩形内，如果不在直接返回false
        if (!bound.containsPoint(point)) {
          return false;
        }
        //如果在外包矩形内则进一步判断
        //该点往右侧发出的射线和矩形边交点的数量,若为奇数则在多边形内，否则在外
        var crossPointNum = 0;
        for (var i = 0; i < pointArray.length; i++) {
          //获取2个相邻的点
          var p1 = pointArray[i];
          var p2 = pointArray[(i + 1) % pointArray.length];
          //如果点相等直接返回true
          if ((p1.lng === point.lng && p1.lat === point.lat) || (p2.lng === point.lng && p2.lat === point.lat)) {
            return true
          }
          //如果point在2个点所在直线的下方则continue
          if (point.lat < Math.min(p1.lat, p2.lat)) {
            continue;
          }
          //如果point在2个点所在直线的上方则continue
          if (point.lat >= Math.max(p1.lat, p2.lat)) {
            continue;
          }
          //有相交情况:2个点一上一下,计算交点
          //特殊情况2个点的横坐标相同
          var crossPointLng;
          if (p1.lng === p2.lng) {
            crossPointLng = p1.lng;
          } else {
            //计算2个点的斜率
            var k = (p2.lat - p1.lat) / (p2.lng - p1.lng);
            //得出水平射线与这2个点形成的直线的交点的横坐标
            crossPointLng = (point.lat - p1.lat) / k + p1.lng;
          }
          //如果crossPointLng的值大于point的横坐标则算交点(因为是右侧相交)
          if (crossPointLng > point.lng) {
            crossPointNum++;
          }

        }
        //如果是奇数个交点则点在多边形内
        return crossPointNum % 2 === 1
      },
      /* 初始化放大 */
      zoomEnd(e){
        const {lng, lat} = e.target.getCenter()
        this.center.lng = lng
        this.center.lat = lat
        this.zoom = e.target.getZoom();
        const markerArrs = this.zoomLevel();
        this.markerArrs = markerArrs;
      },
      zoomLevel(){
        const zoomLevel = this.zoom; //获取地图缩放级别
          if (zoomLevel <= 6) {
            return this.country
          } else if (zoomLevel > 6 && zoomLevel <= 10) {
            return this.province
          } else if (zoomLevel > 10 && zoomLevel <= 13) {
            return this.city
          } else if (zoomLevel > 13 && zoomLevel <= 15) {
            return this.region
          } else if (zoomLevel > 15 && zoomLevel <= 18) {
            return this.street
          } else if (zoomLevel > 18) {
            return this.dot
          } else{
            return [{
              text:'暂无数据',
              long: "116.305434",
              lat: "39.96549",
              count: "0"
            }]
          }
      },
      /* 移动 */
      moveEnd(e){
        const {lng, lat} = e.target.getCenter()
        this.center.lng = lng;
        this.center.lat = lat;
        this.zoom = e.target.getZoom();
      },
      /* 点击marker事件 */
      infoWindowOpen(data,event){
        // 判断当前zoom
        const zoomLevel = this.zoom; //获取地图缩放级别
        const lng=data.long;
        const lat=data.lat;
        if (zoomLevel <= 6) {
            this.zoom = 7;
          } else if (zoomLevel > 6 && zoomLevel <= 10) {
            this.zoom = 11;
          } else if (zoomLevel > 10 && zoomLevel <= 13) {
            this.zoom = 14;
          } else if (zoomLevel > 13 && zoomLevel <= 15) {
           this.zoom = 16;
          } else if (zoomLevel > 15 && zoomLevel <= 18) {
            this.zoom = 19;
          } else if (zoomLevel > 18) {
              this.drawRESULT = [
                data,
              ]
              console.log(this.drawRESULT)
              return
          }
          this.center.lng = lng;
          this.center.lat = lat;
          const markerArrs = this.zoomLevel();
          this.markerArrs = markerArrs;
      },
      /* 新增mockery事件 */
      /* addMarker(data) {
        this.markerArrs = data;
      }, */
      /* 加载第三级数据 */
      /* addLable(data) {

      }, */
      /* 查询结构 */
      searchByStationName(){
        this.searchkeyword = this.formInline.address;
      },
      filterByName(){
        const key = this.mockKeyWord;
      },
      searchcomplete(results){
        if(results){
          this.searchRESULT = results.Ir;
          this.drawRESULT = [];
          this.filterRESULT = [];
          this.mock = true;
        }else{
          this.searchRESULT = [];
          this.mock = false;
        }
        
      },
      /* 显示浮层的信息 to 永生 */
      showAddressDetail(data){
        
      },
      queryData(){ 
        console.log("queryData");
        this.isShowCompanyData = true;
      },
      closeCompanyDataPanel(){
        this.isShowCompanyData = false;
      },

    }
  }
</script>

<style scoped>
@import "./css/style.css";
@import "./css/index.css";
@import "./css/control.css"

</style>

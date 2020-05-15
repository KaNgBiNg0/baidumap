<template>
  <div id="Map_search" class="width15">
    
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
    >
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
      
        <!-- <bm-marker
        :position="{ lng: item.longitude, lat: item.latitude }"
        v-for="(item, index) in markerArrs"
        :key="index"
        @click="infoWindowOpen(item, $event)"
        :title="item.name"
        @mouseover="labelShow = index"
      >
        <bm-label
          :content="item.name"
          :labelStyle="{
            color: 'red',
            fontSize: '14px',
            display: index === labelShow ? 'block' : 'none',
          }"
          :offset="{ width: -5, height: 30 }"
        />
      </bm-marker> -->
        <my-overlay
          :position="{ lng: item.longitude, lat: item.latitude }"
          v-for="(item, index) in markerArrs"
          :key="index"
          :companyData="item"
          :infoWindowOpen="infoWindowOpen"
        >
        </my-overlay>
    </baidu-map>
  
    <div id="mapTools" class="disable">

      <div class="topBlank"></div>
    
      <div class="center">
      
        <div class="leftBlank"></div>

        <div class="content">
          
          <!-- 搜索框 -->
          <searchCompanyInput></searchCompanyInput>

          <!-- 过滤器 -->
          <searchKeyCon></searchKeyCon>

          <!-- 候选单位列表 -->
          <companyListPanel></companyListPanel>

          <!-- 数据结果列表 -->
          <companyDataPanel></companyDataPanel>
        
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
/* import BmOverlay  from 'vue-baidu-map/components/overlays/Overlay.vue' */
import BmLocalSearch from 'vue-baidu-map/components/search/LocalSearch.vue'
import {BmlMarkerClusterer} from 'vue-baidu-map'

import area from './json/ares.json'
import firstData from './json/firstData.json'
import secondData from './json/secondData.json'
import thirdlyData from './json/thirdData.json'
import abc from './json/abc.json'

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

      MyOverlay,
      searchCompanyInput,
      searchKeyCon,

      companyListPanel,
      companyDataPanel
    },
    data(){
      return{
        abc,
        area,
        firstData,
        secondData,
        thirdlyData,
        /* 点聚合 */
        /* stylesInMarker:[
          {
          url, //图片的url 地址必填
          size, // size 图片大小 必填项
          opt_anchor,
          textColor, 
          opt_textSize
           }
        ], */
        active: false,
        /* 界限 */
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
        /* 初始化 */
        map:null,
        center: {lng: 116.404, lat: 39.915},
        zoom: 13,
        markerArrs:[{
          longitude: "116.305434",
          latitude: "39.96549",
          name:'海淀区'
        }],
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
      draw ({BMap, map, el, overlay}) {
       /*  debugger; */
        console.log(BMap, map, el, overlay)
      /* const pixel = map.pointToOverlayPixel(new BMap.Point(116.404, 39.915))
      el.style.left = pixel.x - 60 + 'px'
      el.style.top = pixel.y - 20 + 'px' */
    },
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
        if(this.zoom <= 12){
            alert("请放大到三级数据进行画图找房");
            this.isInDrawing = false;
        }else{
          this.map.clearOverlays()
          this.map.setDefaultCursor('crosshair');
          this.map.addEventListener('mousedown',  (e)=> {
            //如果处于画圈状态下,清空上次画圈的数据结构,设置isMouseDown进入画圈鼠标按下状态

            if(this.isInDrawing){
              this.polyPointArray=[];
              this.isMouseDown = true;
              this.mock= false;
              console.log('jinlaile')
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
        this.addMarker(this.thirdlyData)
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
          let keys = Object.keys(this.thirdlyData)
          for (var i = 0; i < keys.length; i++) {
            //该marker的坐标点
            let name = keys[i];
            let {longitude,latitude} = this.thirdlyData[name];
            var marker = new BMap.Marker(new BMap.Point(longitude,latitude)); // 创建点
	          let markerPoint = marker.getPosition();
            if (this.isPointInPolygon(markerPoint, bound, pointArray)) {
              pointInPolygonArray.push(this.thirdlyData[name])
            }
          }
          if(pointInPolygonArray.length!==0){
            this.markerArrs = pointInPolygonArray;
            this.searchRESULT = [];
            this.filterRESULT = [];
            this.drawRESULT = pointInPolygonArray;
            this.mock = true;
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
        const zoomLevel = this.zoom; //获取地图缩放级别
          console.log(zoomLevel)
          if (zoomLevel <= 13) {
            this.addMarker(this.firstData);
          } else if (zoomLevel > 13 && zoomLevel <= 15) {
            this.addMarker(this.secondData);
          } else if (zoomLevel > 15) {
            this.addMarker(this.thirdlyData)
          }
      },
      /* 移动 */
      moveEnd(e){
        const {lng, lat} = e.target.getCenter()
        this.center.lng = lng;
        this.center.lat = lat;
        this.zoom = e.target.getZoom();
        var zoomLevel = this.zoom; //获取地图缩放级别
        if (zoomLevel > 15) {
            this.addMarker(this.thirdlyData)
        }
      },
      /* 添加点 */
      addoverlay(){
        console.log(12345)
      },
      /* 点击marker事件 */
      infoWindowOpen(data,event){
        // 判断当前zoom
        const zoomLevel = this.zoom; //获取地图缩放级别
        const lng=data.longitude;
        const lat=data.latitude ;
          if (zoomLevel <= 13) {
            this.center.lng = lng;
            this.center.lat = lat;
            this.zoom = 14;
          } else if (zoomLevel > 13 && zoomLevel <= 15) {
            this.center.lng = lng;
            this.center.lat = lat;
            this.zoom = 16;
          } else if (zoomLevel > 15) {
					    /* var mockEle = document.getElementById('mock');
            	if (event.clientX < document.documentElement.clientWidth / 2) {
                mockEle.style.right = "0";
                mockEle.style.left = "auto";
              } else {
                mockEle.style.left = "0";
                mockEle.style.right = "auto";
              } */
              this.mock = true;
              /* this.filterRESULT = [{name:'这里应该是查询的数据！！！点击项，显示项下所有企业信息。'}]
              this.drawRESULT = [];
              this.searchRESULT = []; */
              const dataToShow = [{lat:data.latitude,lng:data.longitude}]
              this.showAddressDetail(dataToShow);
          }
      },
      /* 新增mockery事件 */
      addMarker(data) {
        this.markerArrs = data;
      },
      /* 加载第三级数据 */
      addLable(data) {

      },
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
      /* 显示浮层的信息 目前好像没啥用 */
      showAddressDetail(data){
        let adds=[];
        for (let index = 0; index < data.length; index++) {
          let elementpt = data[index];
          adds.push(elementpt)
        }
        this.adds = adds;
        this.addindex = 0;
        this.bdGEO(0);
      },
      bdGEO() {
        var pt = this.adds[this.addindex];
        //geocodeSearch(pt);
        this.addindex++;
      },
      geocodeSearch(pt) {
        if (index < adds.length - 1) {
          setTimeout(window.bdGEO, 400);
        }
		    myGeo.getLocation(pt, function (rs) {
          var addComp = rs.addressComponents;
          var item =  "商圈:" + rs.business +
            " </br> 地址：" + addComp.province  + addComp.city  + addComp.district  + addComp.street +
            addComp.streetNumber + "";
          console.log(item)
          if(adds.length === 1){
            console.log('title')
            var comAddress = document.getElementById('comAddress')
            comAddress.innerHTML = item;
              /* 通过其查找 企业 */
          const dataAll = [
                {
                  lng:116.313082,lat:40.047674
                },
                {
                  lng:116.328749,lat:40.026922
                },
                {
                  lng:116.347571,lat:39.988698
                },
                {
                  lng:116.316163,lat:39.998333
                },
                {
                  lng:116.313076,lat:40.059011
                },
              ]
              var mockBottom = document.getElementById('mockBottom')
              mockBottom.innerHTML ='';
                showAddressDetail(dataAll);

          }else{
            createEle(item)
          }
        });
      },


    },
  }
</script>

<style scoped>
@import "./css/style.css";
@import "./css/index.css";
/* .sample {
  width: 20px;
  height: 10px;
  line-height: 10px;
  background: rgba(0,0,0,0.5);
  overflow: hidden;
  box-shadow: 0 0 2px #000;
  color: #fff;
  text-align: center;
  padding: 10px;
  position: absolute;
}
.sample.active {
  background: rgba(0,0,0,0.75);
  color: #fff;
} */
  
</style>

<template>
  <bm-overlay
    ref="customOverlay"
    :class="{sample,active,sample2}"
    pane="labelPane"
    @draw="draw">
    <div  @click="infoWindowOpen(companyData, $event)"
      @mouseover="active=true"
      @mouseout="active=false"
      class="content_kang"
      v-show="sample"
    >
      <p>{{companyData.text}}</p>
      <p>共{{companyData.count}}企业</p>
    </div>
    <span v-show="sample2" @click="infoWindowOpen(companyData, $event)">{{companyData.count}}</span>
  </bm-overlay>
</template>

<script>
import BmOverlay  from 'vue-baidu-map/components/overlays/Overlay.vue'
export default {
  components:{
    BmOverlay,
  },
  data(){
    return{
      active:false,
      sample2:false,
      sample:false,
    }
  },
  props: ['text', 'position',"companyData","infoWindowOpen"],
  watch: {
    position: {
      handler () {
        this.$refs.customOverlay.reload()
      },
      deep: true
    }
  },
  methods: {
    draw ({el, BMap, map}) {
    const zoomLevel = map.getZoom();
    if (zoomLevel <= 18) {
          this.sample = true;
          this.sample2 = false;
    } else if (zoomLevel > 18) {
          this.sample2 = true;
          this.sample = false;
    }
      const {lng, lat} = this.position
      const pixel = map.pointToOverlayPixel(new BMap.Point(lng, lat))
      el.style.left = pixel.x - 10 + 'px'
      el.style.top = pixel.y - 10 + 'px'
    }
  }
}
</script>

<style lang="less">
.sample {
  width: 84px;
  height: 84px;
  line-height: 20px;
  font-size: 12px;
  border-radius: 50%;
  background: rgba(0, 167, 91, 0.9);
  overflow: hidden;
  box-shadow: 0 0 2px #000;
  color: #fff;
  text-align: center;
  position: absolute;
}
.sample.active {
  background: rgba(240, 65, 52, 0.9);
  color: #fff;
}
.sample2 {
  width: 31px;
  height: 42px;
  line-height: 20px;
  font-size: 12px;
  color: #fff;
  background: url('./img/ditu.png') no-repeat;
  background-size: 30px 42px;
  text-align: center;
  position: absolute;
  transform: rotate3d(1, 1, 0, -10deg);
}
.sample2 span{
  font-weight: 700;
}
.content_kang{
  margin-top: 16px;
  padding:  0 6px;
  p{
    font-weight: 800;
  }
}
</style>
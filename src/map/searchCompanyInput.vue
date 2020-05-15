<template>
  <div id="searchCompanyInput">
    <input type="text" placeholder="输入企业或区域开始寻找">
    <input type="button" value="查询" @click.stop="search">
    <!-- <div class="searchResult" v-show="true"></div> -->
  </div>
</template>

<script type="text/ecmascript-6">
  export default {
    props:['lags','longs','zoom'],
    data(){
      return{
        inSearch:false,
      }
    },
    method:{
      search(){}
    }
  }
</script>

<style scoped lang="less">
#searchCompanyInput{
  width: 352px;
  height: 40px;
  position: absolute;
  left: 0px;
  top: 0px;
  z-index: 2;
  text-align: center;
  line-height: 40px;
  box-sizing: content-box;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.3);

  input[type=text] {
    text-align: left;
    font: 12px/40px "Simsun";
    color:#999999;
    width:291px;
    display: inline-block;
    height:100%;
    vertical-align: top;
    text-indent:14px;
  }

  input[type=button] {
    width:61px;
    height:100%;
    display: inline-block;
    vertical-align: top;
    background-color: #1492ff;
    color: #ffffff;
    font: 14px/40px "微软雅黑";
    cursor: pointer;
  }
}
// #searchCompanyInput input{
//   color:#fff;
// }
// .searchResult{
//   position: absolute;
//   left: 0;
//   top: 80px;
//   width: 350px;
//   height: 640px;
//   background-color: blueviolet;
//   z-index: 2;
// }

 
</style>

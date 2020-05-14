<template>
  <div id="searchCompanyInput">
    <input type="text" style="width:290px;height:40px;"><input type="button" value="查询" style="width:60px;height:40px;outline:none;background:#1493fa;">
    <div class="searchResult" v-show="true"></div>
  </div>
</template>

<script type="text/ecmascript-6">
  export default {
    data(){
      return{
        inSearch:false,
      }
    }
  }
</script>

<style scoped>
#searchCompanyInput{
  width: 350px;
  height: 42px;
  position: absolute;
  left: 360px;
  top: 100px;
  z-index: 2;
  text-align: center;
  line-height: 40px;
  box-sizing: content-box;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.3);
}
#searchCompanyInput input{
  color:#fff;
}
.searchResult{
  position: absolute;
  left: 0;
  top: 80px;
  width: 350px;
  height: 640px;
  background-color: blueviolet;
  z-index: 2;
}

 
</style>

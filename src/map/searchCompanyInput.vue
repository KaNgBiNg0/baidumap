<template>
  <div id="searchCompanyInput">
    <div class="searchResult" v-show="inSearch"></div>
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
  height: 40px;
  background-color: aqua;
  position: absolute;
  left: 360px;
  top: 100px;
  z-index: 2;
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

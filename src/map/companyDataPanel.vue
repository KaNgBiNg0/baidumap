<template>
  <div class="companyDataPanel" v-show="isShowCompanyData" >
    <iframe class="companyDataIframe" src="http://test.gofusion.cn/dataquery/page/indicator/indicator.jsp?funcId=240792bd64114c43ada687ca5f04c1b6"></iframe>
    <img class="close" @click.stop="closeCompanyDataPanel"/>
  </div>
</template>

<script type="text/ecmascript-6">

  export default {
    props:["list", "isShowCompanyData", "closeCompanyDataPanel"],
    data(){return {
    }},
    methods:{
    },
    watch:{
       isShowCompanyData(val){
        console.log(`isShowCompanyData ===> ${val}`);
        if(val) {
          // 根据list拼接iframe
        } 
       }
    }
  }
</script>

<style scoped lang="less">

  .width15 .companyDataPanel {
    width: 1131px;
  }

  .width12 .companyDataPanel {
    width: 1200px;
  }

  .companyDataPanel {
    pointer-events: auto;
    position: absolute;
    top:0px;
    right: 0px;
    height: 100%;
    background-color: #ffffff;
    z-index: 3;

    .companyDataIframe {
      width: 100%;
      height: 100%;
      z-index: 1;
    }

    .close {
      position: absolute;
      right: -12px;
      top: -12px;
      width: 24px;
      height: 24px;
      background: url(./img/icon.png) no-repeat -318px -35px;
      z-index: 2;
      border-radius: 12px;
    }
  }

</style>

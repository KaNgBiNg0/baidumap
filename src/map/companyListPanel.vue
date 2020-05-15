<template>
  <div class="companyListPanel">
    <div class="tilte">
      <span class="big">企业名称</span><span>共找到</span><span class="number">123</span><span>家企业</span>
      <button>数据查询</button>
    </div>
    <div class="list">
    </div>
    <div class="pages">

    </div>
  </div>
</template>

<script type="text/ecmascript-6">

  export default {
    props:["list"],
    data(){return {
       
    }},
    watch:{
      list(){

      }
    }
  }
</script>

<style lang="less">

.companyListPanel{
  width: 352px;
  height: 100%;
  position: absolute;
  top: 0;
  left: 0;
  z-index: 2;

  .tilte {
    width: 100%;
    height: 53px;
    background: #ddebfc;
    font:12px/58px "Simsun";
    color: #999999;

    span {
      display: inline-block;
      vertical-align: baseline;
    }

    .big{
      font:18px/20px "微软雅黑";
      color: #333333;
      margin: 0 6px 0 18px;
      vertical-align: baseline;
    }

    .number {
      font:20px/20px "Helvetica";
      color: #ff9900;
      margin: 0 5px;
    }
  }
}
 
</style>

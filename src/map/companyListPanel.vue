<template>
  <div class="companyListPanel">
    <div class="tilte">
      <span class="big">企业名称</span><span>共找到</span><span class="number">123</span><span>家企业</span>
      <button class="btn">数据查询</button>
    </div>
    <div class="list">
      <div v-for="(info, i) in showList" :key="i" class="item">
        <span v-html="i+1" class="No"></span>
        <div class="companyInfo">
          <div class="name" >
            <span v-text="info.name"></span>
          </div>
          <div class="info">
            <span class="businessType infos" v-text="info.businessType"></span>
            <span class="staffs infos" v-text="info.staffs"></span>
            <span class="companyType infos" v-text="info.companyType"></span>
          </div>
          <div class="connect">
            <span class="address" v-text="info.address"></span>
            <span class="phone" v-text="info.phone"></span>
          </div>
        </div>
      </div>
    </div>
    <div class="pages">

    </div>
  </div>
</template>
"name":"长安福特长安福特长安福特长安福特长安福特",
          "businessType":"新能源",
          "staffs": "员工人数",
          "companyType":"国企",
          "address":"地址",
          "phone":"电话",
    
          // 数据查询使用
          "code": "组织机构代码"
<script type="text/ecmascript-6">

  export default {
    props:["list"],
    data(){return {
       showList: [{
          "name":"长安福特有限责热公司",
          "businessType":"新能源企业",
          "staffs": "300",
          "companyType":"国企",
          "address":"北京市海淀区中关村",
          "phone":"101010101010",
          "code": "10100101010101"
       },{
          "name":"长安福特有限责热公司",
          "businessType":"新能源企业",
          "staffs": "300",
          "companyType":"国企",
          "address":"北京市海淀区中关村",
          "phone":"101010101010",
          "code": "10100101010101"
       }],
       length: 0,
       index: 0
    }},
    watch:{
      list(){
        // 数据请求
        this.showList = [];
        this.length =  list.length;
        this.index = 0;
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
  display: flex;
  flex-direction: column;
  pointer-events: auto;

  .tilte {
    flex: none;
    width: 100%;
    height: 53px;
    background: #ddebfc;
    font:12px/58px "Simsun";
    color: #999999;
    position: relative;

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

    .btn {
      padding         : 3 20px;
      font-size       : 14px;
      border-radius   : 4px;
      display         : inline-block;
      border          : 0;
      box-shadow      : none;
      line-height     : 31px;
      width           : 76px;
      height          : 31px;
      white-space     : nowrap;
      text-align      : center;
      background: #1493ff;
      color           : #ffffff;
      position      :absolute;
      top :11px;
      right:25px
    }
  }

  .list {
    flex: auto;
    background: #ffffff;
    position: relative;
    
    .item {
      height: 102px;
      width: 313px;
      margin-left:20px;
      border-bottom: 1px solid #e3e8ee;
      
      .No {
        font: 24px/24px "ArialMT";
        color: #ff9e2c;
        margin: 28px 22px auto 6px;
        display:inline-block;
        vertical-align: top;
      }

      .companyInfo {
        width:270px;
        height:103px;
        display: inline-block;


        .name{
          width: 100%;
          height:34px;
          line-height: 34px;

          span {
            vertical-align: bottom;
            font: 18px/18px "微软雅黑";
            color: #333333; 
          }
        }

        .info{
          height:30px;
          line-height: 30px;

          .infos {
            vertical-align: bottom;
            padding: 3px 10px;
            margin-right: 10px;
            font: 12px/12px "Simsun";
            display: inline-block;
          }

          .businessType {
            color: #dcb02e;
            background: #faf3e0;
          }

          .staffs {
            color: #849aae;
            background: #f4f7f9;
          }

          .companyType {
            color: #39becd;
            background: #e1f5f8;
          }
        }

        .connect {
          height:37px;
          line-height: 37px;
        }
      }
    }

  }

  .pages {
    flex: none;
    height: 58px;
    width: 100%;
  }
}
 
</style>

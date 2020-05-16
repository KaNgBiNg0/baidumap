<template>
  <div class="companyListPanel" v-show="list.length>0">
    <div class="tilte">
      <span class="big">企业名称</span><span>共找到</span><span class="number" v-text="list.length"></span><span>家企业</span>
      <button class="btn" @click.stop="queryData">数据查询</button>
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
      <!-- pager-count应该是大于等于5 但是设置5以后宽度溢出 就先改成3 提示waring可以先无视 -->
      <el-pagination class="pagination" background 
			layout="prev, pager, next"
      pager-count="3"
      current-page.sync="index"
			:page-sizes="[10,20,50,100]" 
			:page-size="10"
			:total="list.length"
			@current-change="handleCurrentChange"  
			@size-change="handleSizeChange" 
			>
		</el-pagination>
    </div>
  </div>
</template>
<script type="text/ecmascript-6">

  export default {
    props:["list", "queryData"],
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
       },{
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
       },{
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
       },{
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
       },{
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
       index: 0
    }},
    methods:{
      handleCurrentChange(){
        console.log(arguments)
      },
      handleSizeChange(){}
    },
    watch:{
      list(){
        // 数据请求
        this.showList = [{
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
       },{
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
       },{
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
       },{
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
       },{
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
       }];
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
  background: #ffffff;

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
    position: relative;
    overflow:auto;
    
    .item {
      height: 102px;
      width: 313px;
      margin-left:20px;
      border-bottom: 1px solid #e3e8ee;
      
      .No {
        font: 24px/24px "ArialMT";
        color: #ff9e2c;
        margin: 28px auto auto 6px;
        display:inline-block;
        vertical-align: top;
        display:inline-block;
        width:37px;
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
    width: 312px;
    margin:0 20px;
    display: flex;
    align-items: center;
    justify-content: center;

    .el-pagination{
      width: 312px;
      text-align: center;
    }

    .el-pagination .btn-next,
    .el-pagination .btn-prev,
    .el-pagination .el-pager .number  {
      background: #f5f5f5;
      border: 1px solid #e3e8ee
    }
  }
}
 
</style>

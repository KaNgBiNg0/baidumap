<template>
  <div class="searchKeyCon">
    <div class="item">
      <el-dropdown>
        <span class="el-dropdown-link">
          行业<i class="el-icon-arrow-down el-icon--right"></i>
        </span>
        <el-dropdown-menu slot="dropdown">
          <div class="checkItem">
            <el-checkbox v-model="checked1" @change="change1">备选项1</el-checkbox>
          </div>
          <div class="checkItem">
            <el-checkbox v-model="checked2" @change="change2">备选项2</el-checkbox>
          </div>
          <div class="checkItem">
            <el-checkbox v-model="checked3" @change="change3">备选项3</el-checkbox>
          </div>
          <div class="checkItem">
            <el-checkbox v-model="checked4" @change="change4">备选项4</el-checkbox>
          </div>
        </el-dropdown-menu>
      </el-dropdown>
    </div>
    <div class="item">
      <el-dropdown>
        <span class="el-dropdown-link">
          规模<i class="el-icon-arrow-down el-icon--right"></i>
        </span>
        <el-dropdown-menu slot="dropdown">
          <div class="checkItem">
            <el-checkbox v-model="checked5" @change="change5">备选项5</el-checkbox>
          </div>
          <div class="checkItem">
            <el-checkbox v-model="checked6" @change="change6">备选项6</el-checkbox>
          </div>
          <div class="checkItem">
            <el-checkbox v-model="checked7" @change="change7">备选项7</el-checkbox>
          </div>
          <div class="checkItem">
            <el-checkbox v-model="checked8" @change="change8">备选项8</el-checkbox>
          </div>
        </el-dropdown-menu>
      </el-dropdown>
    </div>
    <div class="item">
      <el-dropdown>
        <span class="el-dropdown-link">
          指标<i class="el-icon-arrow-down el-icon--right"></i>
        </span>
        <el-dropdown-menu slot="dropdown">
          <div class="checkItem">
            <el-checkbox v-model="checked9" @change="change9">备选项9</el-checkbox>
          </div>
          <div class="checkItem">
            <el-checkbox v-model="checked10" @change="change10">备选项10</el-checkbox>
          </div>
        </el-dropdown-menu>
      </el-dropdown>
    </div>
    <div class="item">
        <span class="">
          <i class="el-icon-arrow-down el-icon--right"></i>画圈找企业
        </span>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  export default {
    props:['formInline','searchByStationName',"addtags","removetags"],
    data() {
      return {
        checked1:false,
        checked2:false,
        checked3:false,
        checked4:false,
        checked5:false,
        checked6:false,
        checked7:false,
        checked8:false,
        checked9:false,
        checked10:false,
      };
    },
    methods: {
      change1(val){
        val && this.addtags(1)
        !val && this.removetags(1)
      },
      change2(val){
        val && this.addtags(2)
        !val && this.removetags(2)
      },
      change3(val){
        val && this.addtags(3)
        !val && this.removetags(3)
      },
      change4(val){
        val && this.addtags(4)
        !val && this.removetags(4)
      },
      change5(val){
        val && this.addtags(5)
        !val && this.removetags(5)
      },
      change6(val){
        val && this.addtags(6)
        !val && this.removetags(6)
      },
      change7(val){
        val && this.addtags(7)
        !val && this.removetags(7)
      },
      change8(val){
        val && this.addtags(8)
        !val && this.removetags(8)
      },
      change9(val){
        val && this.addtags(9)
        !val && this.removetags(9)
      },
      change10(val){
        val && this.addtags(10)
        !val && this.removetags(10)
      },
      
    }
  }

</script>

<style  lang='less'>
.searchKeyCon{
  position: absolute;
  top: 0px;
  right: 0px;
  height: 40px;
  width : 377px;
  background: #fff;
  color:#333;
  font: 14px/40px "微软雅黑";
  display: flex;
  flex-direction: row;

  .item {
    flex: 0 0 87px;
    text-align: center;
    vertical-align: middle;
  }

  .item:last-child{
    flex: auto;
  }

  .el-dropdown {
    width: 100%;
    display: inline-block;
    position: relative;
    color: #606266;
    font-size: 14px;
    height: 100%;
    vertical-align: top;
  }
} 
</style>

<style  lang='stylus'>
.con
  .item
    color #333
    font-size 13px
    text-align center
    line-height 50px
    height 50px
    width 85px
    border-right 1px solid #eee
    border-left 1px solid #eee
    margin-left -1px
    .el-icon-arrow-down 
      font-size 12px
.checkItem
  padding 5px 10px
.el-input__inner
  border none 
  &:hover
    border none
</style>

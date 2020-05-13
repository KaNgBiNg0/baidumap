<template>
  <div style="display:flex" class="con">
    <div style="padding: 5px 0;margin-right:5px;border-left:1px solid #eee">
        <el-input placeholder="请输入内容" v-model="formInline.address" class="input-with-select" >
          <i slot="suffix" class="el-input__icon el-icon-search" @click="searchByStationName"></i>
        </el-input>
    </div>
    <div class="item">
      <el-dropdown>
        <span class="el-dropdown-link">
          下拉菜单<i class="el-icon-arrow-down el-icon--right"></i>
        </span>
        <el-dropdown-menu slot="dropdown">
          <div class="checkItem">
            <el-checkbox v-model="checked">备选项</el-checkbox>
          </div>
          <div class="checkItem">
            <el-checkbox v-model="checked">备选项</el-checkbox>
          </div>
          <div class="checkItem">
            <el-checkbox v-model="checked">备选项</el-checkbox>
          </div>
          <div class="checkItem">
            <el-checkbox v-model="checked">备选项</el-checkbox>
          </div>
        </el-dropdown-menu>
      </el-dropdown>
    </div>
    <div class="item">
      <el-dropdown>
        <span class="el-dropdown-link">
          下拉菜单<i class="el-icon-arrow-down el-icon--right"></i>
        </span>
        <el-dropdown-menu slot="dropdown">
          <el-dropdown-item>黄金糕</el-dropdown-item>
          <el-dropdown-item>狮子头</el-dropdown-item>
          <el-dropdown-item>螺蛳粉</el-dropdown-item>
          <el-dropdown-item disabled>双皮奶</el-dropdown-item>
          <el-dropdown-item divided>蚵仔煎</el-dropdown-item>
        </el-dropdown-menu>
      </el-dropdown>
    </div>
    <div class="item">
      <el-dropdown>
        <span class="el-dropdown-link">
          下拉菜单<i class="el-icon-arrow-down el-icon--right"></i>
        </span>
        <el-dropdown-menu slot="dropdown">
          <el-dropdown-item>黄金糕</el-dropdown-item>
          <el-dropdown-item>狮子头</el-dropdown-item>
          <el-dropdown-item>螺蛳粉</el-dropdown-item>
          <el-dropdown-item disabled>双皮奶</el-dropdown-item>
          <el-dropdown-item divided>蚵仔煎</el-dropdown-item>
        </el-dropdown-menu>
      </el-dropdown>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
const cityOptions = ['上海', '北京', '广州', '深圳'];
  export default {
    props:['formInline','searchByStationName'],
    data() {
      return {
        checkAll: false,
        checkedCities: ['上海', '北京'],
        cities: cityOptions,
        isIndeterminate: true,
        checked:false,
      };
    },
    methods: {
      handleCheckAllChange(val) {
        this.checkedCities = val ? cityOptions : [];
        this.isIndeterminate = false;
      },
      handleCheckedCitiesChange(value) {
        let checkedCount = value.length;
        this.checkAll = checkedCount === this.cities.length;
        this.isIndeterminate = checkedCount > 0 && checkedCount < this.cities.length;
      }
    }
  }

</script>

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

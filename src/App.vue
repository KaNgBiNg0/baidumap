<template>
  <div id="app">
    <Map/>
  </div>
</template>

<script>

import Map from './map'
export default {
  name: 'app',
  components: {
    Map
  },
}
</script>

<style lang="less" ref="stylesheet/less" >
#app{
  // width: 100%;
  // height: 100%;
  width: 1920px;
  height: 1080px;
}
</style>

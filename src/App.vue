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

<style lang="stylus" ref="stylesheet/stylus" >
#app{
  width 1920px
  height 1080px
}
</style>

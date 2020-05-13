<template>
  <div>
    <el-tag
      v-for="tag in tags"
      :key="tag.name"
      closable
      size="mini"
      effect="plain"
      type="info"
    >
      {{tag.name}}
    </el-tag>
  </div>
</template>

<script type="text/ecmascript-6">

  export default {
    props:["tags"],
    data(){return {
       
    }},
  }
</script>

<style scoped>

 
</style>

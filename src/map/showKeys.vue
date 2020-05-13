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
    data(){return {
       tags: [
          { name: '标签一', type: '' },
          { name: '标签二', type: 'success' },
          { name: '标签三', type: 'info' },
          { name: '标签四', type: 'warning' },
          { name: '标签五', type: 'danger' }
        ],
    }},
  }
</script>

<style scoped>

 
</style>

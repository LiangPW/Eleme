<!-- 图标插件 -->
<template>
  <span class="classMap" :class="classMap[iconType]"></span>
</template>

<script>
export default {
  name: 'Icon',
  props: {
    iconType: {
      type: Number
    }
  },
  created () {
    // 通过判断seller.supports[i].type的值来动态改变classMap的值
    this.classMap = ['decrease', 'discount', 'guarantee', 'invoice', 'special']
  }
}
</script>

<style scoped>
.classMap{
  display: inline-block;
  background-repeat: no-repeat;
}
</style>

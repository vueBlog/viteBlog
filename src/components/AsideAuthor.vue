<template>
  <div class="author clearfix">
    <div class="item">
      <div class="title">原创</div>
      <div class="number">{{ info.natureZero }}</div>
    </div>
    <div class="item">
      <div class="title">转载</div>
      <div class="number">{{ info.natureOne }}</div>
    </div>
    <div class="item">
      <div class="title">翻译</div>
      <div class="number">{{ info.natureTwo }}</div>
    </div>
    <div class="item">
      <div class="title">views</div>
      <div class="number">{{ info.views }}</div>
    </div>
    <div class="item">
      <div class="title">star</div>
      <div class="number">{{ info.stars }}</div>
    </div>
  </div>
</template>

<script setup>
  const props = defineProps({
    info: {
      type: Object,
      default: () => {}
    }
  })
</script>

<style lang="scss" scoped>
  @import '@/styles/components/aside-author.scss';
</style>

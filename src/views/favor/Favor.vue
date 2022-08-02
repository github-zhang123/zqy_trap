<template>
  <div class="favor" ref="favorRef">
    <h2>Favor 收藏</h2>
    <template v-for="(item) in 100">
      <div>列表数据:{{ item }}</div>
    </template>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import useScroll from '@/hooks/useScroll';

const favorRef = ref()

useScroll(favorRef)

</script>

<style lang="less" scoped>
.favor {
  height: 400px;
  background: skyblue;
  overflow-y: auto;
}
</style>

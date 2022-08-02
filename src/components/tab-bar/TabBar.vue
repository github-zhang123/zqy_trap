<template>
  <div class="tab-bar">
    <van-tabbar v-model="currentIndex" active-color="#ff9645">
      <template v-for="(item, index) in tabbarData">
        <van-tabbar-item :to="item.path">

          <template v-slot:icon>
            <img v-if="currentIndex !== index" :src="getAssetURL(item.image)" alt="">
            <img v-else :src="getAssetURL(item.imageActive)" alt="">
          </template>

          <template v-slot:default>
            <span>{{ item.text }}</span>
          </template>

        </van-tabbar-item>
      </template>
    </van-tabbar>
  </div>
</template>

<script setup>

import { ref, watch } from "vue"
import { useRoute } from "vue-router";
import tabbarData from "@/assets/data/tabbar.js"
import { getAssetURL } from "@/utils/load_assets.js"

const route = useRoute()

const currentIndex = ref(0)
watch(route, (newRoute) => {
  const index = tabbarData.findIndex(item => item.path === newRoute.path)
  if (index === -1) return
  currentIndex.value = index
})


</script>

<style lang="less" scoped>
.tab-bar {

  // 找到类, 对类中的CSS属性重写
  // :deep(.class)找到子组件的类, 让子组件的类也可以生效
  :deep(.van-tabbar-item__icon) {
    font-size: 50px;
  }

  img {
    height: 26px;
  }
}
</style>

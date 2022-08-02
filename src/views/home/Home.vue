<template>
  <div class="home" ref="homeRef">

    <home-nav-bar></home-nav-bar>

    <div class="banner">
      <img src="@/assets/img/home/banner.webp" alt="">
    </div>

    <home-search-box></home-search-box>

    <home-categories></home-categories>

    <div class="search-bar" v-if="isShowSearchBar">
      <search-bar :start-date="'09.19'" :end-date="'09.20'" />
    </div>

    <home-content></home-content>

  </div>
</template>

<script>
export default {
  name: "home"
}
</script>

<script setup>

import { ref, watch, computed, onActivated } from "vue";
import useHomeStore from "@/stores/modules/home";
import HomeNavBar from "./cpns/HomeNavBar.vue"
import HomeSearchBox from "./cpns/HomeSearchBox.vue"
import HomeCategories from "./cpns/HomeCategories.vue"
import HomeContent from "./cpns/HomeContent.vue"
import useScroll from "@/hooks/useScroll"
import SearchBar from "@/components/search-bar/SearchBar.vue"

// 发送网络请求
const homeStore = useHomeStore()
homeStore.fetchHotSuggestData()
homeStore.fetchCategoriesData()
homeStore.fetchHouselistData()

// 监听页面滚动
const homeRef = ref()
const { isReachBottom, scrollTop } = useScroll(homeRef)
watch(isReachBottom, (newValue) => {
  if (newValue) {
    homeStore.fetchHouselistData().then(() => {
      isReachBottom.value = false
    })
  }
})


// 搜索框显示的控制
// const isShowSearchBar = ref(false)
// watch(scrollTop, (newTop) => {
//   isShowSearchBar.value = newTop > 100
// })
const isShowSearchBar = computed(() => {
  return scrollTop.value >= 356
})

// 跳转回home时, 保留原来的位置
onActivated(() => {
  homeRef.value?.scrollTo({
    top: scrollTop.value
  })
})


</script>

<style lang="less" scoped>
.home {
  height: 100vh;
  overflow-y: auto;
  box-sizing: border-box;
  padding-bottom: 60px;
}

.banner {
  img {
    width: 100%;
  }
}

.search-bar {
  position: fixed;
  z-index: 9;
  top: 0;
  left: 0;
  right: 0;
  height: 45px;
  padding: 16px 16px 10px;
  background-color: #fff;
}
</style>

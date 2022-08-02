<template>
  <div class="city top-page">
    <div class="top">
      <!-- 搜索框 -->
      <van-search class="search" v-model="searchValue" shape="round" show-action placeholder="城市/区域/位置"
        @cancel="onCancel" />

      <!-- tab切换 -->
      <van-tabs v-model:active="tabActive" color="#ff9645">
        <template v-for="(value, key, index) in allCities" :key="key">
          <van-tab :title="value.title" :name="key"></van-tab>
        </template>
      </van-tabs>
    </div>
    <div class="content">
      <!-- <city-group :group-data="currentGroup"></city-group> -->
      <template v-for="(value, key, index) in allCities">
        <city-group v-show="tabActive === key" :group-data="value"></city-group>

      </template>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from "vue"
import { storeToRefs } from "pinia";
import { useRouter } from "vue-router"
import useCityStore from "@/stores/modules/city";

import CityGroup from "./cpns/CityGroup.vue"


const router = useRouter()

const searchValue = ref("")
const tabActive = ref()

const onCancel = () => {
  router.back();
}

// 从store中获取数据
const cityStore = useCityStore()
cityStore.fetchAllCitiesData()
const { allCities } = storeToRefs(cityStore)

// 获取选中标签后的数据
// 默认获取的不是响应式，得用 computed
// const currentGroup = computed(() => allCities.value[tabActive.value])

</script>

<style lang="less" scoped>
.city {
  .search {
    --van-search-action-padding: 0 15px 0 8px;

  }

  :deep(.van-field__control) {
    padding: 0 4px;
  }

  .top {
    position: relative;
    z-index: 9;
  }

  .content {
    height: calc(100vh - 98px);
    overflow-y: auto;
  }
}
</style>

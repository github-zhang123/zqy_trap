<template>
  <div class="city-group">

    <van-index-bar :index-list="indexList">
      <van-index-anchor index="热门" />
      <div class="list">
        <template v-for="(city, index) in groupData.hotCities">
          <div class="city" @click="cityClick(city)">{{ city.cityName }}</div>
        </template>
      </div>

      <template v-for="(group, index) in groupData.cities" :key="index">
        <van-index-anchor :index="group.group" />
        <template v-for="(city, indey) in group.cities" :key="indey">
          <van-cell :title="city.cityName" @click="cityClick(city)" />
        </template>
      </template>
    </van-index-bar>
  </div>
</template>

<script setup>
import useCityStore from '@/stores/modules/city';
import { computed } from 'vue';
import { useRouter } from 'vue-router';

// 定义props
const props = defineProps({
  groupData: {
    type: Object,
    default: () => ({})
  }
})

// 动态索引
const indexList = computed(() => {
  const list = props.groupData.cities.map(item => item.group)
  list.unshift("#")
  return list
})

const router = useRouter()
const cityStore = useCityStore()

// 监听城市的点击
const cityClick = (city) => {
  // 选中当前的城市
  cityStore.currentCity = city

  // 返回上一级
  router.back()
}

</script>

<style lang="less" scoped>
.list {
  display: flex;
  justify-content: space-around;
  padding: 10px;
  padding-right: 25px;
  flex-wrap: wrap;

  .city {
    width: 70px;
    height: 28px;
    line-height: 28px;
    text-align: center;
    border-radius: 14px;
    background-color: #fff4ec;
    font-size: 12px;
    color: #000;
    margin: 6px 0;
  }
}
</style>

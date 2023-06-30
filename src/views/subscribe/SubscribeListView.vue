<script lang="ts" setup>
import api from "@/api";
import type { Subscribe } from "@/api/types";
import SubscribeCard from "@/components/cards/SubscribeCard.vue";

// 输入参数
const props = defineProps({
  type: String,
});

// 数据列表
const dataList = ref<Subscribe[]>([]);

// 获取订阅列表数据
const fetchData = async () => {
  try {
    dataList.value = await api.get("subscribe");
  } catch (error) {
    console.error(error);
  }
};

// 加载时获取数据
onMounted(fetchData);

// 过滤数据
const filteredDataList = computed(() => {
  return dataList.value.filter((data) => data.type === props.type);
});
</script>

<template>
  <div class="grid gap-3 grid-subscribe-card">
    <SubscribeCard v-for="data in filteredDataList" :key="data.id" :media="data" />
  </div>
</template>

<style type="scss">
.grid-subscribe-card {
  grid-template-columns: repeat(auto-fill, minmax(20rem, 1fr));
}
</style>
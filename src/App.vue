<template>
  <div
    class="flex flex-col p-4 space-y-4 overflow-auto h-screen"
    @scroll="handleScroll"
  >
    <div
      v-for="(outerList, outerIndex) in loadedData"
      :key="outerIndex"
      class="space-y-2"
    >
      <div class="flex space-x-2">
        <div
          v-for="(innerItem, innerIndex) in outerList"
          :key="innerIndex"
          class="w-12 h-12 flex items-center justify-center border border-gray-300 flex-none rounded-lg cursor-pointer hover:scale-[0.8] transition-transform duration-200"
        >
          {{ innerItem }}
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { computed, onMounted, ref } from "vue";

const itemsCountPerPage = 30;

const data = computed(() => {
  const list: number[][] = [];
  const numberOfOuterLists = Math.floor(Math.random() * 30) + 300;
  for (let i = 0; i < numberOfOuterLists; i++) {
    const innerList: number[] = [];
    const numberOfInnerItems = Math.floor(Math.random() * 20) + 10;
    for (let j = 0; j < numberOfInnerItems; j++) {
      innerList.push(Math.floor(Math.random() * 100));
    }
    list.push(innerList);
  }
  return list;
});

const loadedData = ref<number[][]>([]);

const handleScroll = (event: Event): void => {
  const container = event.target as HTMLElement;
  const { scrollTop, scrollHeight, clientHeight } = container;
  if (scrollTop + clientHeight >= scrollHeight) {
    loadMoreData();
  }
};

const loadMoreData = (): void => {
  const newItems = data.value.slice(
    loadedData.value.length,
    loadedData.value.length + itemsCountPerPage
  );
  loadedData.value.push(...newItems);
};

onMounted(() => {
  loadMoreData();
});
</script>

<template>
  <div class="main-cont">
    <div class="main-cont__h2">
      <h2>Запуски</h2>
    </div>

    <CategoryList
      :categories="categories"
      :activeCategory="activeCategory"
      @select="selectCategory"
    />
    <div class="main-cont__list">
      <ScheduleList :filteredLaunches="filteredLaunches" />
    </div>
  </div>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue';
import { useRoute, useRouter } from 'vue-router';
import CategoryList from '@/components/CategoryList.vue';
import ScheduleList from '@/components/ScheduleList.vue';

const launches = ref([]);
const activeCategory = ref(null);
const route = useRoute();
const router = useRouter();

onMounted(async () => {
  const res = await fetch('https://main.proweb.uz/api/v1/launches/external/course/research/');
  const data = await res.json();
  launches.value = data.results;

  const categoryFromUrl = route.query.category;
  if (categoryFromUrl) {
    activeCategory.value = decodeURIComponent(categoryFromUrl);
  }
});

const categories = computed(() => {
  const map = new Map();
  launches.value.forEach(item => {
    const cat = item.categories?.[0];
    if (cat?.name && cat?.color) {
      map.set(cat.name, cat.color);
    }
  });
  return Array.from(map, ([name, color]) => ({ name, color }));
});

const filteredLaunches = computed(() => {
  if (!activeCategory.value) return launches.value;
  return launches.value.filter(item =>
    item.categories?.some(cat => cat.name === activeCategory.value)
  );
});

function selectCategory(category) {
  if (activeCategory.value === category) {
    activeCategory.value = null;
    router.replace({ query: {} });
  } else {
    activeCategory.value = category;
    router.replace({ query: { category } });
  }
}
</script>

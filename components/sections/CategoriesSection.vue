<template>
  <section class="my-5 grid gap-4 grid-cols-1 sm:grid-cols-2 md:grid-cols-3" :class="limit >= 4 ? 'md:grid-cols-5' : 'md:grid-cols-3'">
    <CategoryCard v-for="category in limitedCategories" :key="category.id" :category="category" />
  </section>
</template>

<script setup>

import { useCategory } from "~/composables/useCategory.js";

const { categories } = useCategory();

const props = defineProps({
  limit: {
    type: Number,
    required: false
  }
});

const limitedCategories = computed(() => categories.slice(0, props.limit));

</script>

<style scoped>
</style>

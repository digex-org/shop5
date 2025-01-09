<template>
  <div class="mb-5">
    <h2 class="text-3xl font-bold" :class="className">{{ header }}</h2>
    <h4>{{ description }}</h4>
  </div>
</template>

<script setup>
  defineProps({
    header: {
      type: String,
      required: true
    },
    description: {
      type: String,
      required: false
    },
    className: {
      type: String,
      required: false
    }
  });
</script>

<style scoped>

</style>
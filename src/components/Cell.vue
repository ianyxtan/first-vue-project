<template>
  <span :class="isPartOfSnake ? 'w-6 h-6 bg-black' : 'w-6 h-6 bg-transparent'">
  </span>
</template>
<script setup>
const props = defineProps(["isPartOfSnake"]);
</script>

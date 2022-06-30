<template>
  <span
    :class="
      isPartOfSnake
        ? 'border w-6 h-6 border-black bg-black'
        : 'border w-6 h-6 border-black bg-transparent'
    "
  >
  </span>
</template>
<script setup>
const props = defineProps(["isPartOfSnake"]);
</script>

<template>
  <span class="w-6 h-6" :class="bgColor"> </span>
</template>
<script setup>
import { computed } from "@vue/runtime-core";

const props = defineProps(["isPartOfSnake", "isApple"]);

const bgColor = computed(() => {
  if (props.isPartOfSnake) {
    return "bg-black";
  } else if (props.isApple) {
    return "bg-red-600";
  } else {
    return "bg-transparent";
  }
});
</script>

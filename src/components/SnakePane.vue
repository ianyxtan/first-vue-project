<template>
  <div class="flex flex-col items-center">
    <div v-for="x in 10" :key="x" class="flex flex-row">
      <Cell v-for="y in 10" :key="y" :isPartOfSnake="isPartOfSnake(x, y)" />
    </div>
  </div>
</template>

<script setup>
import { computed, onMounted, ref } from "vue";
import Cell from "./Cell.vue";

const direction = ref("right");

const snakePosition = ref([
  // x, y
  [1, 1],
  [1, 2],
  [1, 3],
]);

const isPartOfSnake = (x, y) => {
  for (let position of snakePosition.value) {
    if (position[0] === x && position[1] === y) {
      return true;
    }
  }
  return false;
};
</script>

<style scoped>
.active {
  background-color: black;
}
</style>

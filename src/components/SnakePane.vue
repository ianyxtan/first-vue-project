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
const gameInterval = ref(null);

window.addEventListener("keydown", (e) => {
  if (e.key === "ArrowLeft") {
    direction.value = "left";
  } else if (e.key === "ArrowRight") {
    direction.value = "right";
  } else if (e.key === "ArrowUp") {
    direction.value = "up";
  } else if (e.key === "ArrowDown") {
    direction.value = "down";
  } else if (e.key === "Escape") {
    restartGame();
  }
});

const snakePosition = ref([
  // x, y
  [1, 1],
  [1, 2],
  [1, 3],
]);

const restartGame = () => {
  clearInterval(gameInterval.value);
  gameInterval.value = null;
  snakePosition.value = [
    // x, y
    [1, 1],
    [1, 2],
    [1, 3],
  ];
  direction.value = "right";
  startGame();
};

const startGame = () => {
  gameInterval.value = setInterval(moveSnake, 300);
};

const isPartOfSnake = (x, y) => {
  for (let position of snakePosition.value) {
    if (position[0] === x && position[1] === y) {
      return true;
    }
  }
  return false;
};

const gameOver = () => {};

const isOutOfBounds = (x, y) => {
  return x < 1 || x > 10 || y < 1 || y > 10;
};

const moveSnake = () => {
  // determine snake head and index
  let snakeHeadIndex = snakePosition.value.length - 1;
  let snakeHead = snakePosition.value[snakeHeadIndex];

  // remember previous position
  let previousPosition = [...snakeHead];

  // move the snake head
  switch (direction.value) {
    case "right":
      snakeHead[1]++;
      break;
    case "left":
      snakeHead[1]--;
      break;
    case "up":
      snakeHead[0]--;
      break;
    case "down":
      snakeHead[0]++;
      break;
  }

  if (isOutOfBounds(...snakeHead)) {
    return restartGame();
  }

  // move body
  for (let i = snakePosition.value.length - 2; i >= 0; i--) {
    let bodyBodyPartOld = [...snakePosition.value[i]];
    snakePosition.value[i] = [...previousPosition];

    previousPosition = [...bodyBodyPartOld];
  }
};

startGame();
</script>

<style scoped>
.active {
  background-color: black;
}
</style>

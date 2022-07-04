<template>
  <div class="flex flex-col items-center">
    <div class="border border-black">
      <div v-for="x in 10" :key="x" class="flex flex-row">
        <Cell
            v-for="y in 10"
            :key="y"
            :is-part-of-snake="isPartOfSnake(x, y)"
            :is-apple="isApple(x, y)"
        />
      </div>
    </div>
  </div>
</template>

<script setup>
import {computed, onMounted, ref} from "vue";
import Cell from "./Cell.vue";

const direction = ref("right");
const gameInterval = ref(null);
const applePosition = ref(null);
let snakeSpeed = ref(300)

window.addEventListener("keydown", (e) => {
  if (e.key === "ArrowLeft" && direction.value !== "right") {
    direction.value = "left";
  } else if (e.key === "ArrowRight" && direction.value !== "left") {
    direction.value = "right";
  } else if (e.key === "ArrowUp" && direction.value !== "down") {
    direction.value = "up";
  } else if (e.key === "ArrowDown" && direction.value !== "up") {
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
    [1, 1],
    [1, 2],
    [1, 3],
  ];
  direction.value = "right";
  startGame();
};

const startGame = () => {
  generateApple();
  gameInterval.value = setInterval(moveSnake, snakeSpeed.value);
};

const isPartOfSnake = (x, y) => {
  for (let position of snakePosition.value) {
    if (position[0] === x && position[1] === y) {
      return true;
    }
  }
  return false;
};

const isPartOfSnakeBody = (x, y) => {
  let snakeBodyParts = [...snakePosition.value];
  snakeBodyParts.pop();

  for (let position of snakeBodyParts) {
    if (position[0] === x && position[1] === y) {
      return true;
    }
  }

  return false;
}

const isApple = (x, y) => {
  return applePosition.value[0] === x && applePosition.value[1] === y;
};

const generateApple = () => {
  let x = Math.floor(Math.random() * 10 + 1);
  let y = Math.floor(Math.random() * 10 + 1);
  while (isPartOfSnake(x, y)) {
    y = Math.floor(Math.random() * 10 + 1);
    x = Math.floor(Math.random() * 10 + 1);
  }
  applePosition.value = [x, y];
};

const isOutOfBounds = (x, y) => {
  return x < 1 || x > 10 || y < 1 || y > 10;
};

const moveSnake = () => {
  // determine snake head and index
  let snakeHeadIndex = snakePosition.value.length - 1;
  let snakeHead = snakePosition.value[snakeHeadIndex];

  // remember previous position
  let previousPosition = [...snakeHead];
  let isAppleEaten = false

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

  // eats apple
  if (isApple(snakeHead[0], snakeHead[1])) {
    // add points
    isAppleEaten = true
  }

  // if new head eats snake body
  if (isPartOfSnakeBody(...snakeHead)) {
    console.log("body part eaten!")

    return restartGame();
  }

  // move body
  for (let i = snakePosition.value.length - 2; i >= 0; i--) {
    let bodyBodyPartOld = [...snakePosition.value[i]];
    snakePosition.value[i] = [...previousPosition];

    previousPosition = [...bodyBodyPartOld];
  }

  if (isAppleEaten) {
    // generate new apple
    generateApple();

    // make snake longer
    snakePosition.value.unshift(previousPosition);
  }

};

startGame();

</script>

<style scoped>
.active {
  background-color: black;
}
</style>

<template>
  <div class="container">
    <div class="game" id="game"></div>
    <div class="total-score score" draggable="false">{{ totalScore }}</div>
    <div class="add-score score disappear" id="add-socre" draggable="false"></div>
    <div v-if="!isStart" class="flex start">
      <h1>跳一跳</h1>
      <p>2024网页动画课设</p>
      <p>by: Chris Liu</p>
      <button @click="start">PLAY</button>
    </div>
    <div v-if="isOver" class="flex restart">
      <img src="./img/particle.png" alt="" draggable="false">
      <h1 class="score">{{ totalScore }}</h1>
      <p>别伤心，再试一次～</p>
      <button @click="restart">PLAY AGAIN</button>
    </div>
  </div>
</template>

<script setup>
import Game from "./js/game/game.js";
import { onMounted, ref } from 'vue';
var game;

onMounted(() => {
  game = new Game('game')
  game.updateScoreCallback = updateScore;
  game.failCallback = gameOver;
  game.start();
});

const isStart = ref(false);  // game start flag
const isOver = ref(false);  // game over flag
const totalScore = ref(0);  // total score

function updateScore(digit, total) {
  totalScore.value = total;
  var addScore = document.getElementById('add-socre');
  addScore.classList.remove('disappear');
  addScore.style.display = 'block';
  addScore.innerText = '+' + digit;
  setTimeout(() => {
    addScore.classList.add('disappear');
  }, 500);
}

function gameOver(){
  isOver.value = true;
}

function start() {
  isStart.value = true;
  isOver.value = false;
  totalScore.value = 0;
  game.restart();
}

function restart() {
  totalScore.value = 0;
  isStart.value = true;
  isOver.value = false;
  game.restart();
}

</script>

<style scoped>
@font-face {
  font-family: 'NumberFont';
  src: url('res/font/num.ttf') format('truetype')
}

.container {
  width: 100vw;
  height: 100vh;
}

.game {
  width: 100%;
  height: 100%;
  position: absolute;
  top: 0;
  left: 0;
}

.total-score {
  position: absolute;
  top: 5%;
  left: 5%;
  z-index: 10;
}

.add-score {
  display: none;
  position: absolute;
  top: 35%;
  left: 50%;
  transform: translateX(-50%) translateY(-50%);
  z-index: 10;
  opacity: 1;
  transition: all 0.5s;
}

.disappear {
  top: 32%;
  opacity: 0;
}

.score {
  font-family: 'NumberFont';
  font-size: 3rem;
  color: #492a19;
  user-select: none;
}

.flex {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  position: absolute;
  width: 100%;
  height: 100%;
  backdrop-filter: blur(5px);
  z-index: 10;
  color: white;
}

.flex h1 {
  margin-bottom: .5rem;
}

.flex p {
  margin: 0;
}

.flex img {
  position: absolute;
  top: 45%;
  left: 50%;
  transform: translateX(-50%) translateY(-50%);
}

.flex button {
  font-family: 'NumberFont';
  font-size: 2rem;
  margin-top: 5rem;
  padding: 1rem 2rem;
  border: none;
  border-radius: 30px;
  background-color: #f0f0f0;
  color: #333;
  cursor: pointer;
}
</style>

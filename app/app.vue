<template>
  <div>
    <div class="table">
      <div class="ball-shadow" :style="`transform: translate(${ballX + ballZ}px, ${ballY}px)`"></div>
      <div class="line"></div>
      <div class="net"></div>
    </div>
    <Racket :positionX="racket1X + 'px'" :positionY="racket1Y + 'px'" color="#ff0000"  @pointerdown="racket1Active = true" @pointerup="racket1Active = false"/>
    <Racket :positionX="racket2X + 'px'" :positionY="racket2Y + 'px'" color="#0000ff"  @pointerdown="racket2Active = true" @pointerup="racket2Active = false"/>
    <ball :positionX="ballX + 'px'" :positionY="ballY + 'px'" :radius="ballRadius"/>
  </div>
  
</template>

<script setup>
import Racket from '../components/Racket.vue'
import ball from '../components/ball.vue'
const player = ref(1);
const racket1X = ref(100);
const racket1Y = ref(250);
const racket1Active = ref(false);
const ballForRacket1Active = ref(true);
const racket1dx = ref(0);
const racket1dy = ref(0);
const racket2X = ref(100);
const racket2Y = ref(250);
const racket2Active = ref(false);
const ballForRacket2Active = ref(true);
const racket2dx = ref(0);
const racket2dy = ref(0);
const ballX = ref(0);
const ballY = ref(0);
const ballZ = ref(30);
const balldx = ref(0);
const balldy = ref(0);
const balldz = ref(0);
const ballRadius = ref(15);
const isBallMove = ref(false);

const racketMove = (event, p) => {
  if (p == 1){
    if (racket1Active.value){
      racket1dx.value =  event.clientX - 50 - racket1X.value;
      racket1dy.value =  event.clientY - 90 - racket1Y.value;
      if (racket1dx.value > 20) {
        racket1dx.value = 20;
      }
      if (racket1dy.value < -80) {
        racket1dy.value = -80;
      }
      if (racket1dy.value > -40) {
        racket1dy.value = -40;
      }
      racket1X.value = event.clientX - 50;
      racket1Y.value = event.clientY - 90;
    }
  } else {
    if (racket2Active.value){
      racket2dx.value =  event.clientX - 50 - racket2X.value;
      racket2dy.value =  event.clientY - 90 - racket2Y.value;
      if (racket2dx.value > 20) {
        racket2dx.value = 20;
      }
      if (racket2dy.value < -80) {
        racket2dy.value = -80;
      }
      if (racket2dy.value > -40) {
        racket2dy.value = -40;
      }
      racket2X.value = event.clientX - 50;
      racket2Y.value = event.clientY - 90;
    }
  }
  
}


onMounted(() => {
  racket1X.value = window.innerWidth / 2 - 50;
  racket1Y.value = window.innerHeight / 2 + 220;
  racket2X.value = window.innerWidth / 2 - 50;
  racket2Y.value = window.innerHeight / 2 - 420;
  ballX.value = window.innerWidth / 2 - 25;
  ballY.value = window.innerHeight / 2 + 150;

  window.addEventListener('pointermove', e => racketMove(e, player.value));

  setInterval(() => {
    if (ballForRacket1Active.value && (racket1X.value <= ballX.value + ballRadius.value)&&(racket1X.value + 100 >= ballX.value + ballRadius.value)&&(racket1Y.value + 25 <= ballY.value + ballRadius.value)&&(racket1Y.value + 75 >= ballY.value + ballRadius.value)){
      balldx.value += racket1dx.value;
      balldy.value = racket1dy.value;
      balldz.value = 8;
      ballZ.value = 30;
      isBallMove.value = true;
      ballForRacket1Active.value = false;
    }
    if (ballForRacket2Active.value && (racket2X.value <= ballX.value + ballRadius.value)&&(racket2X.value + 100 >= ballX.value + ballRadius.value)&&(racket2Y.value + 75 <= ballY.value + ballRadius.value)&&(racket2Y.value + 125 >= ballY.value + ballRadius.value)){
      balldx.value += racket2dx.value / 2;
      balldy.value = -balldy.value + racket2dy.value;
      balldz.value = 8;
      ballZ.value = 30;
      isBallMove.value = true;
      ballForRacket2Active.value = false;
    }
    if (isBallMove.value){
      balldz.value -= 0.25;
    }
    if (ballY.value > window.innerHeight / 2) {
      ballForRacket2Active.value = true;
    } else {
      ballForRacket1Active.value = true;
    }
    ballZ.value += balldz.value / 10;
    ballX.value += balldx.value / 10;
    ballY.value += balldy.value / 10;
    ballRadius.value = 15 * 180 / (210 - ballZ.value);
    if (ballZ.value <= 0){
      balldz.value = -balldz.value;
      if (Math.abs(ballX.value + ballRadius.value - window.innerWidth / 2) >= 125 || Math.abs(ballY.value + ballRadius.value - window.innerHeight / 2) >=  250){
        balldz.value = 0;
        balldx.value = 0;
        balldy.value = 0;
        ballZ.value = 0;
      }
    }
  }, 10)

})

</script>
<style>
  *{
    scrollbar-width: none;
  }
  .table {
    position: absolute;
    z-index: -1;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    background-color: rgb(0, 193, 61);
    border: 5px solid black;
    width: 250px;
    height: 500px;
    display: flex;
    justify-content: center;
    align-items: center;
    overflow: hidden;
  }
  .net {
    width: 100%;
    height: 5px;
    background-color: white;
    border-top: 2px solid black;
    border-bottom: 2px solid black;
  }
  .line{
    width: 10px;
    height: 100%;
    position: absolute;
    background-color: white;
    border: none;
    z-index: -1;
  }
  .ball-shadow{
    position: absolute;
    left: calc(-50vw + 135px);
    top: calc(-50vh + 260px);
    width: 30px;
    height: 30px;
    border-radius: 50%;
    background-color: rgba(85, 85, 85, 0.462);
    z-index: 1;
  }
</style>
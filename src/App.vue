<script setup>
import { ref } from 'vue'
import GameResult from './components/GameResult.vue'
import GameAlternatives from "./components/GameAlternatives.vue"
import { winnerInfo } from './components/store'

const alternative = ref('')
const compAlternative = ref('')
const winner=ref('')
const round=ref(0)

// Visar info kring poängen
function setWinnerInfo(_winner){
  winner.value = _winner
  round.value++
  if (_winner === 'draw') {
    winnerInfo.value = "No damage dealt"
  } else if (_winner === 'user') {
    winnerInfo.value = 'Player dealt damage'
  } else {
    winnerInfo.value = 'Enemy dealt damage'
  }
}
function setUserAlternative(alt) {
}
function setComputerAlternative(alt) {
}
</script>

<template>
  <h1>Air, Land, Sea</h1>
  <GameAlternatives
  @user-choice="setUserAlternative"
  @computer-choice="setComputerAlternative"
  @winner="setWinnerInfo"
  />
  <h3>{{ winnerInfo }}</h3>

  <GameScore
    :user-alternative="alternative"
    :computer-alternative="compAlternative"
    :winnerInfo="winnerInfo"
    />
  <GameResult :result="{ round,winner}"/>

</template>

<!-- CSS for de lookz-->
<style scoped>
h1{
  position: relative;
  text-align: center;
  min-width: 10px;
  text-shadow: 0.0.10em 0.10em rgb(255, 255, 255);
} 
h3{
  text-align: center;
  text-shadow: 0.0.12em 0.2em lightslategrey;
}
</style>
<script setup>
// Importera funktioner från Vue
import { ref, defineEmits } from 'vue'

// Skapar en instans av emit-funktionen med specifika händelser
const emit = defineEmits(['user-choice', 'computer-choice', 'winner'])

// Skapar en referens till de olika alternativen i spelet
const alternatives = ref(['Anti-Air', 'Helicopter', 'Artillery', 'Paratrooper', 'Tank'])

// Hanterar användarens val av alternativ
function alternativeChosen(e) {
  // Återställ markering för alla knappar
  let buttons = document.getElementsByClassName('button')
  for (let b of buttons) {
    b.classList.remove('vald')
  }
  // Hämtar det valda alternativet och markera det
  let alternative = e.target.innerText
  e.target.classList.add('vald')
  // Skickar ut händelse om användarens val
  emit('user-choice', alternative)
  // Låter datorn göra sitt val
  computerAction()
}

// Funktion för att låta datorn göra ett slumpmässigt val
function computerAction() {
  let compAlternative = alternatives.value[Math.floor(Math.random() * alternatives.value.length)]
  let buttons = document.getElementsByClassName('button')
  for (let b of buttons) {
    b.classList.remove('computerChoice')
    // Markera det valda alternativet för datorn
    if (b.innerText === compAlternative) {
      b.classList.add('computerChoice')
    }
  }
  // Skicka ut händelse om datorns val och avgör vinnare
  emit("computer-choice", compAlternative)
  determineWinner()
}

// Avgör vinnaren baserat på användarens och datorns val
function determineWinner() {
  let userButton, computerButton
  let buttons = document.getElementsByClassName('button')
  for (let b of buttons) {
    // Hämtar användarens val
    if (b.classList.contains('vald')) {
      userButton = b.innerText
    }
    // Hämtar datorns val
    if (b.classList.contains('computerChoice')) {
      computerButton = b.innerText
    }
  }

  // Hittar "index" för användarens och datorns val
  let computerIndex = alternatives.value.indexOf(computerButton)
  let userIndex = alternatives.value.indexOf(userButton)

  // Avgör vinnaren
  if (computerButton === userButton) {
    emit('winner', 'draw')
  } else if (computerIndex % 2 === userIndex % 2) {
    emit('winner', computerIndex > userIndex ? 'computer' : 'user')
  } else {
    emit('winner', computerIndex < userIndex ? 'computer' : 'user')
  }
}
</script>

<template>
  <!-- Visa knappar för varje alternativ -->
  <div id="peter">
    <div v-for="alt in alternatives" @click="alternativeChosen" class="button" :key="alt">
      {{ alt }}
    </div>
  </div>
</template>

<style scoped>
/* bestämmer utseendet för knapparna och vissa element */
#peter {
  display: flex;
  justify-content: center;
  padding-left: 0;
  flex-wrap: wrap;
}
.button {
  text-align: center;
  font-size: larger;
  width: 7em;
  word-wrap: break-word;
  list-style-type: none;
  margin: 0.5em;
  background-color: rgb(150, 200, 220);
  padding: 1em;
  border: 0.3em solid rgb(75, 120, 140);
  border-radius: 1em;
  user-select: none;
  -moz-user-select: none;
  -webkit-user-select: none;
  cursor: pointer;
  color: whitesmoke;
  font-weight: bold;
  text-shadow: 0 0.1em 0.25em rgb(0, 0, 0);
}

.vald {
  background-color: rgb(150, 220, 100);
  border-color: rgba(100, 130, 50);
}
.computerChoice {
  border: 0.3em solid rgb(210, 70, 70);
}

@media screen and (max-width: 700px) {
  ul {
    display: inline;
  }
}
</style>


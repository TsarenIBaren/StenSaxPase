<script setup>
import { defineProps, ref, watch } from 'vue';
import { winnerInfo } from './store';
const prop=defineProps(['result'])
const userScore=ref(0)
const computerScore=ref(0)

watch(prop, () => {
    if (prop.result.winner === 'user') {
        userScore.value++
    } else if (prop.result.winner === 'computer') {
        computerScore.value++
    }
})

// Återställer spelet
function resetGame() {
    userScore.value = 0;
    computerScore.value = 0;
}

</script>

<template>
    <!-- Håller och visar spelarens poäng -->
    <div>
        <span id="userScore">
            <span :class="userScore> computerScore ? 'leader score' : 'score'"> {{ userScore }} </span>
            <p v-if="computerScore > userScore">Player</p>
            <p v-else>Player</p>
        </span>
        <!-- Håller och visar dators poäng -->
        <span id="computerScore">
            <span :class="userScore < computerScore ? 'leader score' : 'score'"> {{ computerScore }} </span>
            <p>Enemy</p>
        </span>
    </div>
    <div><button id="brunbjorn" class="auto" @click="resetGame">Reset</button></div>
</template>


<!-- nedan är bara CSS -->
<style scoped>
#cheesewrap {
    text-align: center;
}

#computerScore, #userScore {
    color: whitesmoke;
    text-shadow: 0.0.25em 0.5em rgb(0, 0, 0);
}

div {
    margin: auto;
    padding: 0.5em;
    width: 50%;
    display: flex;
    justify-content: center;
    text-align: center;

}
.score {
    background-color: rgb(235, 200, 95);
    border: 0.1em solid rgb(145, 120, 45);
    border-radius: 0.35em;
    font-size: 3em;
    display: inline-block;
    text-align: center;
    padding: 0.25em;
    padding-top: 0em;
    padding-bottom: 0em;
    transition: font-size 0.25s ease;
}
p {
    text-align: center;
}
.leader {
    color: rgb(255, 255, 255) (255, 0, 0);
    text-shadow: 0.0.10em 0.25em rgba(255, 255, 255, 0.5);
    font-size: 4em;
    text-align: center;
}
#brunbjorn {
  justify-content: center;
  background-color: rgb(255, 255, 255);
  color: black;
  font-size: larger;
  border-radius: 1em;
}

</style>
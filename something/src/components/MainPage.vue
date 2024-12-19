<template>
  <v-container class="bg-green-lighten-4 pa-3 fill-height" fluid>
    <div>
      <AppBar />
    </div>
    <v-btn @click="generateNumbers" icon="mdi-restart" class="bg-green ma-1 elevation-4"></v-btn>
    <!-- <reset-btn @reset="generateNumbers()"/> -->
    <v-row class="fill-width" no-gutters>
      <v-col cols="1" class="fill-height" height="100">
        <player-area player="Player 1" :cards="players.player1" :length="players.player1.length"
          :winner="winnerStyle(1)" :playerTurn="playerNumber === 1" />
      </v-col>
      <v-col cols="10">
        <v-container class="pa-2">
          <v-responsive class="align-center fill-height mx-auto" max-width="900">
            <transition-group name="card-flip">
              <v-row class="pa-0" no-gutters>
                <v-col v-for="(img, index) in imageArray" :key="index" class="pa-2">
                  <card-component :current="img" :visible="selectResults(index)" @click="toggleSelection(index, img)"
                    :taken="taken(index)" />
                </v-col>
              </v-row>
            </transition-group>
          </v-responsive>
        </v-container>
      </v-col>
      <v-col cols="1">
        <player-area player="Player 2" :cards="players.player2" :length="players.player2.length"
          :winner="winnerStyle(2)" :playerTurn="playerNumber === 2" />
      </v-col>
    </v-row>
  </v-container>
</template>

<script setup>
import { ref, watch } from "vue";
// import images from "../../../Back-end/imageFinder.js"

// const res = await images()

// console.log("images", res);

//players
const players = ref({
  player1: [],
  player2: []
})
const takenCards = ref([])

let playerNumber = ref(1)

const taken = (index) => {
  return takenCards.value.includes(index)
}

const selectedArray = ref({ index: [], images: [] });

const selectResults = (index) => {
  return selectedArray.value.index.includes(index)
}

const validateSelection = ({ images, index }) => {
  if (images[0] === images[1]) {
    players.value[`player${playerNumber.value}`].push(images[0]);
    takenCards.value.push(index[0], index[1]);
  } else {
    playerNumber.value = playerNumber.value === 1 ? 2 : 1;
  }
}

const toggleSelection = (index, img) => {
  if (selectedArray.value.index.includes(index) || takenCards.value.includes(index)) {
    return "Card already taken";
  }

  if (selectedArray.value.index.length < 2) {
    selectedArray.value.index.push(index);
    selectedArray.value.images.push(img);
  }
  if (selectedArray.value.index.length >= 2) {
    setTimeout(() => {
      validateSelection(selectedArray.value);
      selectedArray.value.index = [];
      selectedArray.value.images = [];
    }, 1000);

  }
};

const restartGame = () => {
  players.value.player1 = [];
  players.value.player2 = [];
  takenCards.value = [];
  playerNumber.value = 1;
  selectedArray.value.index = [];
  selectedArray.value.images = [];

  generateNumbers();

};

const winnerStyle = (number) => {
  return players.value[`player${number}`].length > players.value[`player${number === 1 ? 2 : 1}`].length
}

watch(players.value, (newVal)=>{
  console.log(newVal);
  console.log(newVal.player1.length + newVal.player2.length, imageArray.value.length/2);

  if ((newVal.player1.length + newVal.player2.length) >= imageArray.value.length) {
    console.log("🚽🚽🚽🚽🚽🥹⛑️🚹");
    setTimeout(() => {
      restartGame();
    }, 1000)

  }
})


// create random places
const imageArray = ref([]);
const duplicates = [];
const randomNumberGenerator = () => {
  const number = Math.ceil(Math.random() * 18);
  if (imageArray.value.includes(number)) {
    if (!duplicates.includes(number)) {
      imageArray.value.push(number);
      duplicates.push(number);
    }
  } else {
    imageArray.value.push(number);
  }
}

const generateNumbers = () => {
  imageArray.value = [];
  duplicates.length = 0;
  for (let index = 0; index < 300; index++) {
    randomNumberGenerator();
  };
}

generateNumbers();


</script>

<style scoped>
.card-flip-enter-active,
.card-flip-leave-active {
  transition: all 0.5s;
}

.card-flip-enter-from,
.card-flip-leave-to {
  opacity: 0;
  transform: scale(0.8);
}

.card-flip-enter-to,
.card-flip-leave-from {
  opacity: 1;
  transform: scale(1);
}
</style>
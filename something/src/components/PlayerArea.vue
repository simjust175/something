<template>
  <v-card class="fill-width d-flex flex-column align-center pb-4 pt-2"
    :class="{ turnStyle: props.playerTurn, notTurn: !props.playerTurn }">
    <div class="fill-width bg-grey-lighten-2 d-flex justify-center">
      <h4>{{ props.player }}</h4>
    </div>

    <v-spacer class="pb-4 bg-blue"></v-spacer>

    <div class="text-grey-darken-1 bg-white px-2 rounded elevation-3" :class="{ winnerBorder: winner }">
      <h1>{{ props.length }}</h1>
    </div>

    <v-spacer class="pb-4 bg-blue"></v-spacer>

    <div v-if="props.cards.length > 0">
      <transition-group name="fade">
      <v-img height="70" width="70" class="my-3 ma-1 elevation-6 border-4" v-for="(img, index) in props.cards" :key="index"
        :src="cardImages(img)" cover></v-img>
      </transition-group>
    </div>

    

  </v-card>
</template>

<script setup>
import { defineProps } from "vue";

const props = defineProps({
  player: String,
  cards: Array,
  length: Number,
  winner: Boolean,
  playerTurn: Boolean
})

const cardImages = (img) => `../../public/images/${img}.jpeg`

</script>

<style>
.full-height {
  height: 100%;
}

.fill-width {
  width: 100%;
}

.flex-column {
  flex-direction: column;
}

.align-center {
  align-items: center;
}

.mx-2 {
  margin-left: 0.5rem;
  /* Adjust as needed */
  margin-right: 0.5rem;
  /* Adjust as needed */
}

.winnerBorder {
  border: 5px, rgb(4, 174, 4), solid;
  background: rgba(111, 178, 111, 0.767);
  color: rgb(4, 192, 4);
}

.turnStyle {
  background: rgba(63, 225, 63, 0.511);
  border: rgba(47, 177, 47, 0.991), 4px, solid;
}

.notTurn {
  background: rgba(187, 202, 184, 0.863);
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s;
}

.fade-enter,
.fade-leave-to {
  opacity: 0;
}
</style>
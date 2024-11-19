<template>
  <v-card class="border-thin elevation-4" height="120" width="120" :id="props.current" >
    <audio ref="flipSound" src="../../public/sounds/flipSound.mp3"></audio>
    <div class="card-inner" :class="{ flipped: !props.visible, invisible: props.taken }">
      <v-img :src="currentImg" height="120" width="120" class="front" cover></v-img>
      <v-img src="/images/cover.jpeg" height="120" width="120" class="back" cover></v-img>
    </div>
  </v-card>
</template>

<script setup>
import { ref, computed, defineProps, toRefs, watch } from "vue";

const props = defineProps({
  current: Number,
  visible: Boolean,
  taken: Boolean
});

const { flipSound } = toRefs({
  flipSound: ref(null),
});

const currentImg = computed(() => `/images/${props.current}.jpeg`);

const playFlipSound = () => {
  if (props.visible) {
    flipSound.value.play();
  }
}

watch(() => props.visible, () => {
  playFlipSound();
});
</script>

<style scoped>
.invisible {
  display: none;
  background: transparent;
}
.card-inner {
  position: relative;
  width: 100%;
  height: 100%;
  transform-style: preserve-3d;
  transition: transform 0.5s;
}

.front,
.back {
  position: absolute;
  width: 100%;
  height: 100%;
  backface-visibility: hidden;
}

.back {
  transform: rotateY(180deg);
}

.flipped {
  transform: rotateY(180deg);
}
</style>
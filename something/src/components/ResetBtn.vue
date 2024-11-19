<template>
  <v-btn append-icon="mdi-refresh" @click="emit('restart')">
    Restart
  </v-btn>
</template>

<script setup>
import { defineEmits } from "vue";

const emit = defineEmits(["restart"]);



</script>

<style>

</style>
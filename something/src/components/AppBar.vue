<template>
    <v-app-bar color="green" 
    image="../../public/images/logo.jpeg"
    dark>
    <template v-slot:image>
          <v-img
            gradient="to top right, green-darken-1, green-lighten-1"
          ></v-img>
    </template>
        <v-img height="40" src="/images/logo.png" class="ml-0 pl-0 align-start"></v-img>
  
      <v-app-bar-title>Memor-imal</v-app-bar-title>
  
      <v-spacer></v-spacer>
    </v-app-bar>
  </template>
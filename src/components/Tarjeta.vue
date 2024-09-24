<template>
    <RouterLink to = '/'>Regresemos</RouterLink>
    <v-app id="inspire">
    <v-layout>
      <v-flex xs12 sm6 offset-sm3>
    <v-card>
          <v-img
            src="https://cdn.vuetifyjs.com/images/cards/desert.jpg"
            aspect-ratio="2.75"
          ></v-img>
  
          <v-card-title primary-title>
            <div>
              <h3 class="headline mb-0">Kangaroo Valley Safari</h3>
              <div> {{ card_text }} </div>
            </div>
          </v-card-title>
  
          <v-card-actions>
            <v-btn flat color="orange" @click=irAFoto()>Share</v-btn>
            <v-btn flat color="orange">Explore</v-btn>
          </v-card-actions>
        </v-card>
    </v-flex>
    </v-layout>
  </v-app>
  </template>

  <script>
  import {ref} from "vue";
import { RouterLink } from "vue-router/dist/vue-router.prod.cjs";
  const show = ref(false);
  function irAFoto(){
    window.open(google.com); }
  </script>
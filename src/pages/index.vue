<template>
  <v-app id="app">
    <v-container fluid>
      <div class="text-center">
      <h1 class="display-1">Vue Speech Input</h1>
      <p class="pa-2">*Compatible only with Chrome and Firefox</p>
      <p class="pa-2">Press the mic to start recording...</p>
      <img :src="require('@/assets/logo.png')" height="100">
      </div>
      
      <v-layout row wrap justify-center class="mt-4 pa-2">
        <!-- Input Text Field -->
        <v-flex xs12 sm10 text-xs-center>
          <v-textarea
            label="Speech to Text"
            v-model="text"
            textarea
            outlined
          ></v-textarea>
        </v-flex>
        <!-- Speech Input Component -->
        <v-flex xs12 sm8 md4 text-xs-center>
          <SpeechInput :text.sync="text" en-US @speechend="speechEnd" />
        </v-flex>
      </v-layout>

    </v-container>
  </v-app>
</template>

<script>
import SpeechInput from "@/components/SpeechInput"

export default {
  name: 'Home',
  components: {
    SpeechInput
  },
  data () {
    return {
      text: '',
      sentences: null
    }
  },
  methods: {
    speechEnd ({sentences, text}) {
      console.log('text', text)
      console.log('sentences', sentences)
      this.sentences = sentences
    }
  }
}
</script>

<style>
#app {
  background-color: #FAFAFA;
}
</style>

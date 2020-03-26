<template>
  <v-app id="app">
    <v-container fluid>
      <v-layout row wrap justify-center class="mt-4">
        <h1 class="display-1">Vue Speech Input</h1>
        <img src="@/assets/logo.png">
        <!-- Input Text Field -->
        <v-flex xs12 sm10 text-xs-center>
          <v-text-field
            label="Speech to Text"
            v-model="text"
            textarea
            outlined
          ></v-text-field>
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

</style>
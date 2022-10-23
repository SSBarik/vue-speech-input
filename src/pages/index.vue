<template>
  <v-app id="app">
    <v-container fluid>
      <div class="text-center">
        <h1 class="display-1">Vue Speech Input</h1>
        <p class="pa-2">Press the mic to start recording...</p>
        <img :src="require('@/assets/logo.png')" height="100" />
      </div>

      <!-- Speech Input Component -->
      <SpeechInput :text.sync="text" en-US @speechend="speechEnd" />
      
    </v-container>

    <div id="browser-support" class="pa-2 overline">
      <span>
        <a
          href="https://developer.mozilla.org/en-US/docs/Web/API/SpeechRecognition#browser_compatibility"
          >Check compatibile browsers
        </a>
      </span>
    </div>
  </v-app>
</template>

<script>
import SpeechInput from "@/components/SpeechInput";

export default {
  name: "Home",
  components: {
    SpeechInput,
  },
  data() {
    return {
      text: "",
      sentences: null,
    };
  },
  methods: {
    speechEnd({ sentences, text }) {
      console.log("text", text);
      console.log("sentences", sentences);
      this.sentences = sentences;
    },
  },
};
</script>

<style>
#app {
  background-color: #fafafa;
}

#browser-support {
  position: fixed;
  right: 0px;
  bottom: 0px;
  width: auto;
  height: auto;
  background-color: #f1f1f1;
  border-radius: 10px 0 0 0;
}
</style>
<template>
  <div>
    <v-textarea
      v-if="!error"
      label="Speech to Text"
      v-model="text"
      textarea
      outlined
    ></v-textarea>

    <v-container class="speechCard">
      <v-card max-width="400px" class="speechCard">
        <v-card-text>
          <v-layout row wrap justify-center class="mt-4 pa-2">
            <v-flex xs8 sm9 text-xs-center>
              <!-- error notification -->
              <p v-if="error" class="red--text">{{ error }}</p>
              <!-- Run time Text in speech box -->
              <p v-else class="mb-0">
                <span>{{ runtimeTranscription }}</span>
              </p>
            </v-flex>
            <!-- On Off Toggle Btn -->
            <v-flex xs2 sm1 text-xs-center>
              <v-btn
                dark
                @click.stop="
                  toggle ? endSpeechRecognition() : startSpeechRecognition()
                "
                icon
                :color="!toggle ? 'grey' : speaking ? 'red' : 'red darken-3'"
                :class="{ 'animated infinite pulse': toggle }"
              >
                <!-- <v-icon>{{ toggle ? 'mdi-download' : 'Start Recording' }}</v-icon> -->
                <v-icon v-if="toggle">mdi-microphone-off</v-icon>
                <v-icon v-else>mdi-microphone</v-icon>
              </v-btn>
            </v-flex>
          </v-layout>
        </v-card-text>
      </v-card>
    </v-container>
  </div>
</template>

<script>
let SpeechRecognition =
  window.SpeechRecognition || window.webkitSpeechRecognition;
let recognition = SpeechRecognition ? new SpeechRecognition() : false;

export default {
  name: "SpeechInput",
  props: {
    lang: {
      type: String,
      default: "en-US",
    },
    text: {
      type: [String, null],
      required: false,
    },
  },
  data() {
    return {
      error: false,
      speaking: false,
      toggle: false,
      runtimeTranscription: "",
      sentences: [],
    };
  },
  methods: {
    checkCompatibility() {
      if (!recognition) {
        this.error =
          "Speech Recognition is not available on this browser. Please use a supported browser";
        console.log(this.error);
      }
    },
    endSpeechRecognition() {
      recognition.stop();
      this.toggle = false;
      this.$emit("speechend", {
        sentences: this.sentences,
        text: this.sentences.join(". "),
      });
    },
    startSpeechRecognition() {
      if (!recognition) {
        this.error =
          "Speech Recognition is not available on this browser. Please use a supported browser";
        return false;
      }
      this.toggle = true;
      recognition.lang = this.lang;
      recognition.interimResults = true;

      recognition.addEventListener("speechstart", (event) => {
        this.speaking = true;
      });

      recognition.addEventListener("speechend", (event) => {
        this.speaking = false;
      });

      recognition.addEventListener("result", (event) => {
        const text = Array.from(event.results)
          .map((result) => result[0])
          .map((result) => result.transcript)
          .join("");
        this.runtimeTranscription = text;
      });

      recognition.addEventListener("end", () => {
        if (this.runtimeTranscription !== "") {
          this.sentences.push(
            this.capitalizeFirstLetter(this.runtimeTranscription)
          );
          this.$emit(
            "update:text",
            `${this.text}${this.sentences.slice(-1)[0]}. `
          );
        }
        this.runtimeTranscription = "";
        recognition.stop();
        if (this.toggle) {
          // keep it going.
          recognition.start();
        }
      });
      recognition.start();
    },
    capitalizeFirstLetter(string) {
      return string.charAt(0).toUpperCase() + string.slice(1);
    },
  },
  mounted() {
    this.checkCompatibility();
  },
};
</script>

<style>
.speechCard {
  justify-content: center;
  align-content: center;
  align-items: center;
  text-align:center;
}

</style>
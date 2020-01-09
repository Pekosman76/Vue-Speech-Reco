<template>
  <div>
    <div class="speech-to-txt" @click="startTxtToSpeech">Speech to txt</div>
    <div class="speech-transciption">
      <div v-for="(word, index) in transcription_" :key="index">
        {{ word }}
      </div>
      <div>{{ runtimeTranscription_ }}</div>
    </div>

    <div class="txt-to-speech" @click="startSpeechToTxt">Txt to speech</div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      runtimeTranscription_: "",
      transcription_: [],
      lang_: "fr-FR"
    };
  },
  methods: {
    startTxtToSpeech() {
      // initialisation of voicereco
      window.SpeechRecognition =
        window.SpeechRecognition || window.webkitSpeechRecognition;
      const recognition = new window.SpeechRecognition();
      recognition.lang = this.lang_;
      recognition.interimResults = true;

      // event current voice reco word
      recognition.addEventListener("result", event => {
        const text = Array.from(event.results)
          .map(result => result[0])
          .map(result => result.transcript)
          .join("");
        this.runtimeTranscription_ = text;
      });

      // end of transcription
      recognition.addEventListener("end", () => {
        this.transcription_.push(this.runtimeTranscription_);
        this.runtimeTranscription_ = "";
        recognition.stop();
      });
      recognition.start();
    },
    startSpeechToTxt() {
      // start speech to txt
      var utterance = new SpeechSynthesisUtterance("Message Envoyé");
      window.speechSynthesis.speak(utterance);
    }
  }
};
</script>

<style lang="scss">
.speech-to-txt,
.txt-to-speech {
  display: grid;
  width: 200px;
  height: 100px;
  border-radius: 20px;
  border: 2px solid grey;
  background-color: rgb(248, 245, 245);
  font-size: 38px;
  color: black;
  font-family: Arial, Helvetica, sans-serif;
  place-items: center;
}
.speech-transciption {
  width: 500px;
  padding: 20px;
  border: 2px solid grey;
  background-color: rgb(211, 228, 253);
  border-radius: 20px;
}
</style>

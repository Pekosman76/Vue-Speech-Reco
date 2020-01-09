# Vue-Speech-Reco
Simple vue js way speech to text and text to speech

### Created two button with methods for speech to text and text to speech

```html
<div class="speech-to-txt" @click="startTxtToSpeech">Speech to txt</div>
<div class="txt-to-speech" @click="startSpeechToTxt">Txt to speech</div>

```
### Configuration for speech language


```js
 data() {
   return {
     runtimeTranscription_: "",
     transcription_: [],
     lang_: "fr-FR"
   };
 },
``` 

### Add Methods for start speech to text and text to speech


```js
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
``` 

### Speech documentation
https://developer.mozilla.org/fr/docs/Web/API/SpeechRecognition
https://developer.mozilla.org/fr/docs/Web/API/Window/speechSynthesis


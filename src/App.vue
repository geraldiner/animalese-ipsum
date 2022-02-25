<template>
  <div class="container">
    <Header />
    <OptionsForm @on-change="onChange" @send-options="generateIpsum" />
    <GeneratedResult v-show="paragraphs.length > 0" :paragraphs="paragraphs" />
  </div>
</template>

<script>
import Header from "./components/Header.vue";
import OptionsForm from "./components/OptionsForm.vue";
import GeneratedResult from "./components/GeneratedResult.vue";

export default {
  name: "App",
  components: {
    Header,
    OptionsForm,
    GeneratedResult,
  },
  data() {
    return {
      paragraphs: [],
    };
  },
  methods: {
    onChange() {
      this.paragraphs = [];
    },
    async generateIpsum(options) {
      const { pCount, pLength } = options;
      const catchphrases = await this.fetchCatchphrases();
      catchphrases.shift();

      const paras = [];
      for (let i = 0; i < pCount; i++) {
        paras.push(this.generateParagraph(catchphrases, pLength));
      }
      this.paragraphs = paras;
    },
    generateParagraph(catchphrases, pLength) {
      const allowedSentencesRanges = {
        long: [12, 15],
        medium: [6, 11],
        short: [3, 5],
      };
      let sentences = [];
      const min = allowedSentencesRanges[pLength][0];
      const max = allowedSentencesRanges[pLength][1];
      const sentenceCount = this.getRandomNum(min, max);
      for (let i = 0; i < sentenceCount; i++) {
        sentences.push(this.generateSentence(catchphrases));
      }
      return sentences.join(" ");
    },
    generateSentence(catchphrases) {
      let words = [];

      const wordCount = this.getRandomNum(5, 10);
      for (let i = 0; i < wordCount; i++) {
        let index = Math.floor(catchphrases.length * Math.random());
        words.push(catchphrases[index]);
      }
      words[0] = words[0][0].toUpperCase() + words[0].slice(1);
      return words.join(" ") + ".";
    },
    getRandomNum(min, max) {
      return Math.floor(min + (max - min) * Math.random());
    },
    async fetchCatchphrases() {
      const res = await fetch(
        "https://raw.githubusercontent.com/geraldiner/acnh-message-usen/master/String_USen/Npc/STR_NNpcPhrase.csv"
      );
      const txt = await res.text();

      const data = txt
        .trim()
        .split("\n")
        .map((text, i) => {
          const word = text.split(",")[1];
          return word.slice(1, -1);
        });
      return data;
    },
  },
};
</script>

<style>
</style>

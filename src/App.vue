<template>
  <h1>Animalese Ipsum Generator</h1>
  <h3>Animal Crossing-themed Lorem Ipsum Generator</h3>
  <form @submit="generateIpsum" id="options">
    <section class="p-count">
      <label for="p-count">
        <input @change="setPCount" id="p-count" type="number" :value="pCount" />
        Paragraphs
        <section class="p-count-nav">
          <span @click="increment" class="p-count-button p-count-up">+</span>
          <span @click="decrement" class="p-count-button p-count-down">-</span>
        </section>
      </label>
    </section>

    <section class="p-length">
      <label for="p-length-long">
        <input
          @click="setPLength"
          id="p-length-long"
          type="radio"
          name="long"
          value="long"
          checked
          v-model="pLength"
        />
        Long
      </label>
      <label for="p-length-medium">
        <input
          @click="setPLength"
          id="p-length-medium"
          type="radio"
          name="p-length"
          value="medium"
          v-model="pLength"
        />
        Medium
      </label>
      <label for="p-length-small">
        <input
          @click="setPLength"
          id="p-length-small"
          type="radio"
          name="p-length"
          value="short"
          v-model="pLength"
        />
        Short
      </label>
    </section>
    <section class="extras">
      <label for="">
        <input type="checkbox" id="" /> Something something extra
      </label>
      <label for="">
        <input type="checkbox" id="" /> Other something extra
      </label>
    </section>
    <section class="submit">
      <button type="submit">Submit</button>
    </section>
  </form>
  <section class="result" v-for="(p, i) in paragraphs" :key="i">
    <p>{{ p }}</p>
  </section>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      paragraphs: [],
      pCount: 5,
      pLength: "long",
      allowedSentencesRanges: {
        long: [12, 15],
        medium: [6, 11],
        short: [3, 5],
      },
      catchphrases: [],
    };
  },
  methods: {
    async generateIpsum(e) {
      e.preventDefault();
      const data = await this.fetchCatchphrases();
      data.shift();
      this.catchphrases = data;

      const paras = [];
      for (let i = 0; i < this.pCount; i++) {
        paras.push(this.generateParagraph());
      }
      this.paragraphs = paras;
    },
    generateParagraph() {
      let sentences = [];
      const min = this.allowedSentencesRanges[this.pLength][0];
      const max = this.allowedSentencesRanges[this.pLength][1];
      const sentenceCount = Math.floor(min + (max - min) * Math.random());
      for (let i = 0; i < sentenceCount; i++) {
        sentences.push(this.generateSentence());
      }
      return sentences.join(" ");
    },
    generateSentence() {
      let words = [];
      const min = 5;
      const max = 10;
      const wordCount = Math.floor(min + (max - min) * Math.random());
      for (let i = 0; i < wordCount; i++) {
        let index = Math.floor(this.catchphrases.length * Math.random());
        words.push(this.catchphrases[index]);
      }
      return words.join(" ") + ".";
    },
    increment() {
      this.pCount += 1;
    },
    decrement() {
      this.pCount -= 1;
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

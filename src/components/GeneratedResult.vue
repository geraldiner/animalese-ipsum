<template>
  <section class="result">
    <button @click="copyToClipboard" id="copy">Copy to Clipboard</button>
    <p v-for="(p, i) in paragraphs" :key="i">{{ p }}</p>
  </section>
</template>

<script>
export default {
  name: "GeneratedResult",

  props: {
    paragraphs: Array,
  },
  methods: {
    copyToClipboard(e) {
      e.preventDefault();
      const ipsum = this.paragraphs.join("\n\n");
      navigator.clipboard.writeText(ipsum);
    },
  },
};
</script>

<style scoped>
.result {
  background-color: #bc834b;
  padding: 25px;
  word-wrap: break-word;
  border-radius: 8px;
  box-shadow: 2px 2px 2px 2px #41220b;
}

.result p {
  color: #fff !important;
}

#copy {
  border: none;
  background: #f7c758;
  color: #41220b;
}

#copy:hover {
  background: #e0a246;
}
</style>
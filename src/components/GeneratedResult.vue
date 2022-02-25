<template>
  <section class="result">
    <p v-for="(p, i) in paragraphs" :key="i">{{ p }}</p>
  </section>
</template>

<script>
export default {
  name: "GeneratedResult",
  props: {
    paragraphs: Array,
  },
};
</script>

<style scoped>
.result {
  background-color: #bc834b;
  padding: 25px;
  word-wrap: break-word;
  border-radius: 8px;
}

.result p {
  color: #fff !important;
}
</style>
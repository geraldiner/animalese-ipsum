<template>
  <form @change="onChange" @submit="onSubmit" id="options">
    <section class="p-count">
      <h6>Number of Paragraphs:</h6>
      <label for="p-count">
        <input
          @change="updatePCount"
          id="p-count"
          type="number"
          min="1"
          max="10"
          :value="options.pCount"
        />
        <section class="p-count-nav">
          <span @click="increment" class="p-count-button p-count-up">+</span>
          <span @click="decrement" class="p-count-button p-count-down">-</span>
        </section>
        <span class="p-count-label">Paragraphs</span>
      </label>
    </section>

    <section class="p-length">
      <h6>Paragraph Length:</h6>
      <section class="labels">
        <label for="p-length-long">
          <input
            id="p-length-long"
            type="radio"
            name="long"
            value="long"
            checked
            v-model="options.pLength"
          />
          Long
        </label>
        <label for="p-length-medium">
          <input
            id="p-length-medium"
            type="radio"
            name="p-length"
            value="medium"
            v-model="options.pLength"
          />
          Medium
        </label>
        <label for="p-length-small">
          <input
            id="p-length-small"
            type="radio"
            name="p-length"
            value="short"
            v-model="options.pLength"
          />
          Short
        </label>
      </section>
    </section>
    <!--     <section class="extras">
      <h6>Extras:</h6>
      <label for="">
        <input type="checkbox" id="" /> Something something extra
      </label>
      <label for="">
        <input type="checkbox" id="" /> Other something extra
      </label>
    </section> -->
    <section class="submit">
      <button type="submit">Submit</button>
    </section>
  </form>
</template>

<script>
export default {
  name: "OptionsForm",
  data() {
    return {
      options: {
        pCount: 5,
        pLength: "long",
      },
    };
  },
  methods: {
    increment() {
      if (this.options.pCount >= 10) return;
      this.options.pCount += 1;
      this.$emit("on-change");
    },
    decrement() {
      if (this.options.pCount <= 0) return;
      this.options.pCount -= 1;
      this.$emit("on-change");
    },
    updatePCount(e) {
      e.preventDefault();
      this.options.pCount = Number(e.target.value);
    },
    onSubmit(e) {
      e.preventDefault();

      this.$emit("send-options", this.options);
    },
    onChange(e) {
      e.preventDefault();
      this.$emit("on-change");
    },
  },
  emits: ["send-options", "on-change"],
};
</script>

<style scoped>
form {
  background: #fff;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 2px 2px 2px 2px rgba(0, 0, 0, 0.5);
}

.submit {
  display: flex;
  justify-content: center;
}

.submit button {
  width: 75%;
}

input:not([type="checkbox"]):not([type="radio"]):not([type="range"]) {
  height: 42px;
}
.p-count {
  position: relative;
}

input[type="number"]::-webkit-inner-spin-button,
input[type="number"]::-webkit-outer-spin-button {
  -webkit-appearance: none;
  margin: 0;
}

input[type="number"] {
  -moz-appearance: textfield;
}

.p-count input {
  width: 75px;
  height: 42px;
  line-height: 1.65;
  float: left;
  display: block;
  padding: 0;
  margin: 0;
  padding-left: 20px;
  border: 1px solid #eee;
}

.p-count input:focus {
  outline: 0;
}

.p-count-nav {
  float: left;
  position: relative;
  height: 42px;
}

.p-count-button {
  position: relative;
  cursor: pointer;
  border-left: 1px solid #eee;
  width: 20px;
  text-align: center;
  color: #333;
  font-size: 13px;
  font-family: "Trebuchet MS", Helvetica, sans-serif !important;
  line-height: 1.7;
  -webkit-transform: translateX(-100%);
  transform: translateX(-100%);
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  -o-user-select: none;
  user-select: none;
}

.p-count-button.p-count-up {
  position: absolute;
  height: 50%;
  top: 0;
  border-bottom: 1px solid #eee;
}

.p-count-button.p-count-down {
  position: absolute;
  bottom: -1px;
  height: 50%;
}

.p-count-label {
  display: inline-block;
  display: flex;
  align-items: center;
  height: 50px;
  padding-left: 25px;
}

.p-length .labels {
  display: flex;
  justify-content: space-around;
}
</style>
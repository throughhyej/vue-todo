<template>
  <div>
    <label name="todo-input" />
    <input id="todo-input" :value="item" @input="handleInput" />
    <button @click="addTodo" type="button">add</button>
  </div>
</template>

<script lang="ts">
import Vue from "vue";

export default Vue.extend({
  props: {
    item: {
      type: String,
      required: true,
    },
  },
  methods: {
    addTodo() {
      this.$emit("add");
    },
    handleInput(event: InputEvent) {
      const inputEvent = event.target as HTMLInputElement;
      this.$emit("input", inputEvent.value);
    },
  },
});
</script>

<style scoped></style>

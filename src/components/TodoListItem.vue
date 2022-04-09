<template>
  <li>
    <span class="item" :class="todoClass" @click="toggleItem(idx)">
      {{ todo.title }}
    </span>
    <button @click="removeItem(idx)">del</button>
  </li>
</template>

<script lang="ts">
import Vue, { PropType } from "vue";
import { Todo } from "@/App.vue";

export default Vue.extend({
  props: {
    todo: Object as PropType<Todo>,
    idx: Number,
  },
  computed: {
    todoClass(): string | null {
      return this.todo.done ? "complete" : null;
    },
  },
  methods: {
    toggleItem() {
      this.$emit("toggle", this.todo, this.idx);
    },
    removeItem(idx: number) {
      // @click="$emit("remove", todo)"으로 작성시, 테스트 mocking 시 곤란함으로 메서드 추천
      this.$emit("remove", idx);
    },
  },
});
</script>

<style scoped>
.item {
  cursor: pointer;
}
.complete {
  text-decoration: line-through;
}
</style>

<template>
  <div>
    <header>
      <div>Vue To-do with Typescript</div>
    </header>
    <main>
      <TodoInput :item="todoText" @input="updateTodoInput" @add="addTodo" />
      <ul>
        <TodoListItem
          v-for="(todo, idx) in todoItems"
          :todo="todo"
          :key="idx"
          :idx="idx"
          @remove="removeItem"
          @toggle="toggleItem"
        ></TodoListItem>
      </ul>
    </main>
  </div>
</template>

<script lang="ts">
import Vue from "vue";
import TodoInput from "./components/TodoInput.vue";
import TodoListItem from "./components/TodoListItem.vue";

const STORAGE_KEY = "vue-todo-ts-v1";
const storage = {
  save(todoItems: Todo[]) {
    const parsed = JSON.stringify(todoItems);
    localStorage.setItem(STORAGE_KEY, parsed);
  },
  fetch(): Todo[] {
    const todoItmes = localStorage.getItem(STORAGE_KEY) || "[]";
    const result = JSON.parse(todoItmes);
    return result;
  },
};

export interface Todo {
  title: string;
  done: boolean;
}

export default Vue.extend({
  components: {
    TodoInput,
    TodoListItem,
  },
  data() {
    return {
      todoText: "",
      todoItems: [] as Todo[],
    };
  },
  methods: {
    updateTodoInput(value: string) {
      this.todoText = value;
    },
    addTodo() {
      // localStorage.setItem(value, value);
      const value: Todo = {
        title: this.todoText,
        done: false,
      };
      this.todoItems.push(value);
      this.save();
      this.initInput();
    },
    initInput() {
      this.todoText = "";
    },
    fetchTodoItems() {
      this.todoItems = storage.fetch();
    },
    removeItem(idx: number) {
      this.todoItems.splice(idx, 1);
      this.save();
    },
    toggleItem(todo: Todo, idx: number) {
      this.todoItems.splice(idx, 1, {
        // title: todo.title,
        // 기존값 재배치 위함 (spread)
        ...todo,
        done: !todo.done,
      });
      this.save();
    },
    save() {
      // 등록 시 정렬되게 위치 수정
      this.todoItems.sort((a, b) => {
        if (a.title < b.title) return -1;
        else if (a.title > b.title) return 1;
        else return 0;
      });
      storage.save(this.todoItems);
    },
  },
  created() {
    this.fetchTodoItems();
  },
});
</script>

<style scoped></style>

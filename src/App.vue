<template>
  <div class="container-app">
    <form class="wrapper-input" @submit.prevent="addTodo()">
      <input type="text" class="input-todo" v-model="inputValue" />
      <button type="submit" class="btn-add">Add</button>
    </form>
    <div class="wrapper-todo-content">
      <TodoItem
        v-for="todo in todos"
        :todoValue="todo.value"
        :isDone="todo.isDone"
        :todos="todos"
        :id="todo.id"
        @toggleTodo="toggleTodo"
        @removeTodo="removeTodo"
        @updateTodo="updateTodo"
      />
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";
import TodoItem from "./components/TodoItem.vue";

const inputValue = ref("");
const todos = ref([]);

function addTodo() {
  todos.value.push({ value: inputValue.value, isDone: false, id: Date.now() });
  inputValue.value = "";
}

function toggleTodo(id) {
  todos.value = todos.value.map((todo) => {
    if (todo.id === id) {
      return { ...todo, isDone: !todo.isDone };
    }
    return { ...todo };
  });
}

function removeTodo(id) {
  console.log(todos.value.filter((todo) => todo.id !== id));
  todos.value = todos.value.filter((todo) => todo.id !== id);
}

function updateTodo(id, newTodo) {
  console.log({ id, newTodo });
  todos.value = todos.value.map((todo) => {
    if (todo.id === id) {
      return { ...todo, value: newTodo };
    }
    return todo;
  });
}
</script>

<style scoped>
.container-app {
  max-width: 600px;
  margin: auto;
  display: flex;
  flex-direction: column;
  row-gap: 0.75rem;
  padding: 1rem 0;
}

.wrapper-input {
  display: flex;
}

.input-todo {
  width: 100%;
  padding: 0.5rem;
  border-radius: 0.5rem 0 0 0.5rem;
}

.btn-add {
  border-radius: 0 0.5rem 0.5rem 0;
  border: 0;
  background-color: aquamarine;
  padding: 0.5rem;
}

.wrapper-todo-content {
  display: flex;
  flex-direction: column;
  row-gap: 0.5rem;
}
</style>

<template>
  <div class="todo" :class="isDone ? 'todo-done' : 'todo-not-done'">
    <form
      action=""
      v-if="isEditActive"
      class="form-input-edit"
      @submit.prevent="
        () => {
          isEditActive = false;
          updateDefaultEditValue();
          $emit('updateTodo', id, editValue);
        }
      "
    >
      <input type="text" class="input-edit" v-model="editValue" />
      <button type="submit" style="display: none"></button>
    </form>
    <p v-else @click="handleClickToggle(id)" class="todo-text">{{ todoValue }}</p>
    <div class="action">
      <img src="/trash-bin-delete-svgrepo-com.svg" width="20" height="20" @click="handleClickDelete(id)" class="icon" />
      <img src="/marker-color-colour-svgrepo-com.svg" width="20" height="20" @click="handleClickEdit(id)" class="icon" />
    </div>
  </div>
</template>
<script setup>
import { ref, defineProps } from "vue";

const props = defineProps(["todoValue", "isDone", "todos", "id"]);
const emit = defineEmits(["toggleTodo", "removeTodo", "updateTodo"]);

const isEditActive = ref(false);
const editValueDefault = ref(props.todoValue);
const editValue = ref(props.todoValue);

console.log(props);
console.log(editValue.value);

function handleClickDelete(id) {
  emit("removeTodo", id);
}

function updateDefaultEditValue() {
  editValueDefault.value = props.todoValue;
}

function handleClickEdit(id) {
  console.log(editValue.value);
  if (isEditActive.value) {
    editValue.value = editValueDefault.value;
  }
  isEditActive.value = !isEditActive.value;
}

function handleClickToggle(id) {
  console.log(id);
  emit("toggleTodo", id);
}
</script>
<style scoped>
.todo-not-done {
  background-color: rgb(227, 227, 227);
}
.todo-done {
  background-color: rgb(241, 241, 241);
  color: #c1c1c1;
}

.todo {
  cursor: pointer;
  padding: 1rem;
  border-radius: 0.5rem;
  display: flex;
  justify-content: space-between;
  column-gap: 0.5rem;
}

.action {
  display: flex;
  column-gap: 0.5rem;
}

.todo-text {
  flex: 1;
}

.form-input-edit {
  width: 100%;
}
.input-edit {
  padding: 0.5rem;
  width: 95%;
  border-radius: 0.5rem;
}
.icon {
  margin: auto;
}
</style>

<template>
  <div class="container">
    <h1 class="display-6 text-center fw-bolder text-success my-4">Todo App</h1>
    <TodoInput @add-todo="handleAddTodo" />
    <TodoList :todos="todos" @show-modal="handleShowModal" @remove-todo="handleRemoveTodo" />
  </div>
  <EditModal
    :isEditing="isEditing"
    :selectedTodo="selectedTodo"
    @update-todo="handleUpdateTodo"
    @close-modal="handleCloseModal"
  />
</template>

<script setup>
import { ref } from "vue";
import TodoInput from "./components/TodoInput.vue";
import TodoList from "./components/TodoList.vue";
import EditModal from "./components/EditModal.vue";

const todos = ref([]);
const isEditing = ref(false);
const selectedTodo = ref(null);

const handleAddTodo = (newTodo) => {
  if (todos.value.some((todo) => todo.title === newTodo)) return;
  else
    todos.value.push({
      title: newTodo,
      status: "to-do",
    });
};
const handleShowModal = (todo, index) => {
  selectedTodo.value = { ...todo, index };
  isEditing.value = true;
};
const handleCloseModal = () => {
  isEditing.value = false;
  selectedTodo.value = null;
};
const handleUpdateTodo = (updatedTodo) => {
  const { title, status, index } = updatedTodo;
  todos.value[index] = {
    ...todos.value[index],
    title,
    status,
  };
};
const handleRemoveTodo = (index) => {
  todos.value.splice(index, 1);
};
</script>

<style scoped>
.cursor-pointer {
  cursor: pointer;
}
.td-500 {
  min-width: 300px;
  white-space: normal;
  word-wrap: break-word;
  overflow-wrap: break-word;
}
.select {
  font-size: 14px;
}
</style>

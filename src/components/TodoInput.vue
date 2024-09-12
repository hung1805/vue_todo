<template>
  <form @submit.prevent="handleEmitInput">
    <input type="text" placeholder="Enter a new todo" v-model="inputValue" />
    <button type="submit">Add Todo</button>
  </form>
</template>

<script setup>
import { ref } from "vue";

const emit = defineEmits(["add-todo"]);
const inputValue = ref("");
const handleEmitInput = () => {
  if (inputValue.value.trim() === "") return alert("NEED CONTENT!!!");
  else {
    emit("add-todo", inputValue.value.trim());
    inputValue.value = "";
  }
};
</script>

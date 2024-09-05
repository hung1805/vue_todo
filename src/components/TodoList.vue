<script setup>
import { ref } from "vue";

const data = ref([
  { id: 1, title: "todo1", isCompleted: true },
  {
    id: 2,
    title: "todo2",
    isCompleted: false,
  },
]);
const newTodo = ref("");
const addTodo = () => {
  if (newTodo.value.trim() !== "") {
    data.value.push({
      id: data.value.length + 1,
      title: newTodo.value,
      isCompleted: false,
    });
    newTodo.value = "";
    console.log(data.value);
  } else {
    alert("Need content!");
  }
};
const removeTodo = (index) => {
  data.value.splice(index, 1);
};
</script>
<template>
  <div>
    <h1>TO DO LIST</h1>
    <input type="text" v-model="newTodo" placeholder="new todo" />
    <button v-on:click="addTodo">ADD</button>
    <ul v-if="data.length > 0">
      <li v-for="(todo, index) in data" :key="todo.id">
        <span>{{ index + 1 }}</span>
        <input type="checkbox" v-model="todo.isCompleted" />
        <p :class="{ completed: todo.isCompleted }">{{ todo.title }}</p>
        <button @click="removeTodo(index)">Delete</button>
      </li>
    </ul>
    <p v-else>No todo!</p>
  </div>
</template>
<style scoped>
.todo-list {
  max-width: 400px;
  margin: 0 auto;
  text-align: center;
}

input[type="text"] {
  width: 100%;
  padding: 10px;
  margin-bottom: 20px;
  box-sizing: border-box;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 10px 0;
}

.completed {
  text-decoration: line-through;
  color: #111;
}
</style>

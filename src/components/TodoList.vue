<script setup>
import { defineProps, defineEmits } from "vue";

const props = defineProps({
  todos: {
    type: Array,
    required: true,
  },
});

const emit = defineEmits(["show-modal", "remove-todo"]);
const editTodo = (todo, index) => {
  emit("show-modal", todo, index);
};
const removeTodo = (index) => {
  emit("remove-todo", index);
};
</script>
<template>
  <div class="container">
    <table v-if="todos.length > 0" class="table table-bordered mt-5">
      <thead>
        <tr class="text-center">
          <th scope="col">Task</th>
          <th scope="col">#</th>
          <th scope="col">#</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(todo, index) in todos" :key="todo.title">
          <td class="td-500 align-content-center">{{ todo.title }}</td>
          <td class="text-center">
            <span @click="editTodo(todo, index)" class="px-2 py-1 bg-success rounded-1 cursor-pointer"
              ><i class="fa fa-pen"></i
            ></span>
          </td>
          <td class="text-center" @click="removeTodo(index)">
            <span class="px-2 py-1 bg-danger rounded-1 cursor-pointer"><i class="fa fa-trash"></i></span>
          </td>
        </tr>
      </tbody>
    </table>
    <h3 v-else class="mt-3 text-center text-danger">No todo!</h3>
  </div>
</template>
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

<script setup>
import { ref, onMounted, watch } from "vue";

const data = ref([]);
const statusArr = ["to-do", "in-progress", "finished"];
const newTodo = ref("");
const isEdit = ref(false);
const indexEdit = ref(null);
const editStatus = ref({ index: null, status: "" });
const addTodo = () => {
  if (newTodo.value.trim() !== "") {
    if (isEdit.value) {
      data.value[indexEdit.value].title = newTodo.value;
      indexEdit.value = null;
      isEdit.value = false;
    } else {
      data.value.push({
        id: data.value.length + 1,
        title: newTodo.value,
        status: "to-do",
      });
    }
    newTodo.value = "";
  } else {
    alert("Need content!");
  }
};
const removeTodo = (index) => {
  data.value.splice(index, 1);
};
const editTodo = (index) => {
  isEdit.value = true;
  indexEdit.value = index;
  newTodo.value = data.value[index].title;
  const submitBtn = document.querySelector("#submitBtn");
  submitBtn.innerHTML = "UPDATE";
};
const changeStatus = (index) => {
  editStatus.value.index = index;
};

const handleChangeStatus = () => {
  if (editStatus.value.status) {
    data.value[editStatus.value.index].status = editStatus.value.status;
    editStatus.value = {
      index: null,

      status: "",
    };
  } else return;
};
const loadTodos = () => {
  const savedTodos = localStorage.getItem("todos");
  if (savedTodos) {
    data.value = JSON.parse(savedTodos);
  }
};
const saveTodos = () => {
  localStorage.setItem("todos", JSON.stringify(data.value));
};
onMounted(() => {
  loadTodos();
});
watch(
  data,
  () => {
    saveTodos();
  },
  { deep: true }
);
</script>
<template>
  <div class="container">
    <h1 class="display-6 text-center fw-bolder text-success my-4">TO DO LIST</h1>
    <form class="d-flex" @submit.prevent="addTodo">
      <input type="text" v-model="newTodo" class="form-control" placeholder="Enter text" />
      <button id="submitBtn" type="submit" class="btn btn-primary rounded-1 fw-semibold">ADD</button>
    </form>
    <table v-if="data.length > 0" class="table table-bordered mt-5">
      <thead>
        <tr class="text-center">
          <th scope="col">Task</th>
          <th scope="col">Status</th>
          <th scope="col">#</th>
          <th scope="col">#</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(todo, index) in data" :key="todo.id">
          <td class="td-500 align-content-center">{{ todo.title }}</td>
          <td class="td-500 d-flex justify-content-between align-items-center">
            {{ todo.status }}
            <span @click="changeStatus(index)" class="d-block text-end">
              <i class="fa-solid fa-pen-to-square"></i>
            </span>
            <span v-if="editStatus.index === index">
              <form @submit.prevent="handleChangeStatus" class="d-flex">
                <select v-model="editStatus.status" class="form-select" :class="{ select: true }">
                  <option
                    v-for="status in statusArr.filter((item) => item !== todo.status)"
                    :key="status"
                    :value="status"
                  >
                    {{ status }}
                  </option>
                </select>
                <button type="submit" class="btn btn-outline-primary">OK</button>
              </form>
            </span>
          </td>
          <td class="text-center">
            <span @click="editTodo(index)" class="px-2 py-1 bg-success rounded-1 cursor-pointer"
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

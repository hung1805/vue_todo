<template>
  <div
    v-if="isEditing && selectedTodo"
    id="editModal"
    class="modal fade show"
    :class="[isEditing ? 'd-flex' : '']"
    tabindex="-1"
    role="dialog"
    aria-labelledby="edit-modal"
  >
    <div class="modal-dialog" role="document">
      <div class="modal-content">
        <div class="modal-header">
          <h3 class="modal-title fw-bold">Update Todo</h3>
        </div>
        <div class="modal-body">
          <div>
            <input type="text" v-model="title" />
          </div>
          <div>
            <select name="status" id="status">
              <option v-for="sta in statusArr" :key="sta" :value="sta">{{ sta }}</option>
            </select>
          </div>
        </div>
        <div class="modal-footer">
          <button class="btn btn-primary" @click="saveChange">Save</button>
          <button class="btn btn-primary" @click="closeModal">Close</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { defineProps, ref, watch, defineEmits } from "vue";
const props = defineProps({
  isEditing: {
    type: Boolean,
    required: true,
  },
  selectedTodo: {
    type: Object || null,
  },
});

const emit = defineEmits(["update-todo", "close-modal"]);
const title = ref(props.selectedTodo ? props.selectedTodo.title : "");
const status = ref(props.selectedTodo ? props.selectedTodo.status : "");
const statusArr = ["to-do", "in-progress", "finished"];

//watch for change in prop selectedTodo to update local state
watch(
  () => props.selectedTodo,
  (newTodo) => {
    if (!newTodo) return;
    title.value = newTodo.title;
    // status.value = newTodo.status;
  },
  { immediate: true }
);
const saveChange = () => {
  emit("update-todo", {
    ...props.selectedTodo,
    status: status.value,
    title: title.value,
  });
  emit("close-modal");
};
const closeModal = () => {
  emit("close-modal");
};
</script>

<style></style>

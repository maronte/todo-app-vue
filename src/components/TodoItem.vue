<script setup>
import { defineProps, reactive, defineEmits } from "vue";
const props = defineProps({
  id: {
    type: Number,
    default: 0,
  },
  text: {
    type: String,
    default: "",
  },
  done: {
    type: Boolean,
    default: false,
  },
});
const todoModel = reactive({
  id: props.id,
  text: props.text,
  done: props.done,
});

const emit = defineEmits(["remove", "update"]);
const remove = () => {
  emit("remove", todoModel.id);
};
const update = () => {
  emit("update", todoModel);
};
</script>
<template>
  <li :key="todoModel.id" :class="todoModel.done ? 'done' : 'undone'">
    <input type="checkbox" v-model="todoModel.done" @click="update" />
    {{ todoModel.text }}
    <button class="remove" @click="remove">x</button>
  </li>
</template>
<style>
.remove {
  color: red;
  border-radius: 5px;
  cursor: pointer;
}

.remove:hover {
  background-color: #e74c3c;
  color: beige;
}

.done {
  text-decoration: line-through;
}
</style>

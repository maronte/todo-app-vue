<script setup>
import { ref, computed } from "vue";
import TodoItem from "./components/TodoItem.vue";

const todoList = [];

const newTodo = ref("");
const todos = ref(todoList);
const showAll = ref(true);

const addTodo = () => {
  todos.value.push({
    id: new Date().getTime(),
    text: newTodo.value,
    done: false,
  });
  newTodo.value = "";
};
const removeTodo = (id) => {
  todos.value = todos.value.filter((todo) => todo.id !== id);
};
const updateTodo = (todo) => {
  const index = todos.value.findIndex((t) => t.id === todo.id);
  todos.value[index] = todo;
};
const remove = (done = true) => {
  todos.value = done ? todos.value.filter((todo) => !todo.done) : [];
};

const todosFiltered = computed(() => {
  return showAll.value ? todos.value : todos.value.filter((todo) => !todo.done);
});

const toggleShowAll = () => {
  showAll.value = !showAll.value;
};
</script>

<template>
  <div class="container">
    <h1 class="title">TODOLIST</h1>
    <form @submit.prevent="addTodo">
      <input type="text" v-model="newTodo" placeholder="Add a new todo" />
      <button>Add</button>
    </form>
    <div>
      <ul>
        <TodoItem
          v-for="todo in todosFiltered"
          :key="todo.id"
          :id="todo.id"
          :text="todo.text"
          :done="todo.done"
          @remove="(id) => removeTodo(id)"
          @update="(todo) => updateTodo(todo)"
        />
      </ul>
    </div>
    <div>
      <button @click="remove">Clear Done</button>
      <button @click="remove(false)">Clear All</button>
      <button @click="toggleShowAll">
        {{ showAll ? "Hide Done" : "Show Done" }}
      </button>
    </div>
  </div>
</template>

<style>
* {
  box-sizing: border-box;
  background-color: #42b883;
}

.container {
  margin: 5em auto;
  max-width: 600px;
  padding: 20px;
  background: aliceblue;
  border: 4px solid #35495e;
  border-radius: 10px;
}

.container * {
  background: inherit;
}

.title {
  font-size: 1.5em;
  text-align: center;
  margin-bottom: 1em;
}
</style>

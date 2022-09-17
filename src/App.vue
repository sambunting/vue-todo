<script setup>
import { ref, onMounted } from 'vue';
import Todo from './Todo.vue';
import Input from './Input.vue';

const todos = ref([]);

/**
 * The name of the localstorage item to use
 */
const localStorageItem = 'todos'

/**
 * Gets the todos from the localstorage, and returns the parsed todos 
 */
const getTodos = () => {
  return JSON.parse(localStorage.getItem(localStorageItem));
}

/**
 * Saves todos in localstorage
 *
 * @param {object[]} items Array of todos to save
 */
const saveTodos = (items) => {
  localStorage.setItem(localStorageItem, JSON.stringify(items));
}

/**
 * Create a todo, and save it
 * 
 * @param {string} value The title/text of the todo
 */
const createTodo = (value) => {
  // Get all of the existing todos, and save them, but this time add our new todo onto the end.
  const updated = [...getTodos(), { text: value, completed: false }];
  saveTodos(updated);

  // Once a todo has been created, update the state for the app.
  todos.value = updated;
}

/**
 * Update a todo, and save it to localstorage
 *
 * @param {string} text The text of the todo item
 * @param {boolean} value The checked value of the todo
 */
const updateTodo = (text, value) => {
  // First get all of the existing todos
  const existing = getTodos();

  // Find the item we're going to update
  const index = existing.findIndex((x) => x.text = text);

  // Set the completed value of the index we have found
  existing[index].completed = value;

  // Save the changes
  saveTodos(existing);
}

/**
 * Method called when the application is mounted/loaded
 */
onMounted(() => {
  // If the todo item in the session storage - initialise it.
  if (!getTodos()) {
    saveTodos([]);
  }

  // When the app loads, set the todo items.
  todos.value = getTodos();
})

</script>

<template>
  <Input @submit="createTodo" />

  <ul>
    <li v-for="todo in todos">
      <Todo :text="todo.text" :completed="todo.completed" @update="updateTodo"></Todo>
    </li>
  </ul>
</template>

<style scoped>

</style>

<script setup>
import { ref } from 'vue';

const emit = defineEmits(["add"])

const newTodo = ref("")

const handleSubmit = () => {
  emit("add", newTodo.value)
  newTodo.value = ""
}
</script>

<template>
  <form @submit.prevent="handleSubmit">
    <input v-model="newTodo" type="text" placeholder="Enter new note...">
    <input type="submit" value="Add todo">
  </form>
</template>

<style scoped>
form {
  margin-top: 1rem;
  display: flex;
  justify-content: center;
  flex-direction: column;
  width: 100%;
}

input[type="submit"] {
  border: none;
  outline: none;
  padding: .7rem 2rem;
  background: #999;
  color: white;
  text-transform: uppercase;
  transition: 0.2s ease;
  margin: 1rem 0;
}

input[type="submit"]:hover {
  background: #bbb;
  cursor: pointer;
}

input[type="text"] {
  border: none;
  outline: none;
  border-bottom: 2px solid #333;
  background: transparent;
  padding: .75rem;
  font-size: 1rem;
}
</style>
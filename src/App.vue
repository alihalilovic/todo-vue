<script setup>
import { computed, onBeforeMount, ref } from "vue"
import TodoForm from "./components/TodoForm.vue"
import TodoItem from "./components/TodoItem.vue"

let id = 0

const todos = ref([])
const hideCompleted = ref(false)

onBeforeMount(() => {
  const todoList = JSON.parse(window.localStorage.getItem("todoList"))
  if (!todoList) return
  todos.value = todoList
})

const addTodo = (newTodo) => {
  console.log(newTodo)
  todos.value.push({
    text: newTodo,
    checked: false,
    id: ++id
  })

  window.localStorage.setItem("todoList", JSON.stringify(todos.value))

}

const handleClickTodo = (todo) => {
  todos.value = todos.value.map(t => {
    if (t === todo) {
      return {
        ...todo,
        checked: !todo.checked
      }
    }

    return t
  })

  window.localStorage.setItem("todoList", JSON.stringify(todos.value))
}

const filteredTodos = computed(() => {
  if (hideCompleted.value) {
    return todos.value.filter(todo => !todo.checked)
  }

  return todos.value
})

const deleteTodo = (todo) => {
  todos.value = todos.value.filter(t => t != todo)
}

const deleteSelectedTodos = () => {
  todos.value = todos.value.filter(t => !t.checked)
}

</script>

<template>
  <div class="container">
    <TodoForm @add="payload => addTodo(payload)" />

    <div class="todo-container">
      <ul class="todo-list">
        <TodoItem :todos="filteredTodos" @todoClick="payload => handleClickTodo(payload)"
          @todoDelete="payload => deleteTodo(payload)" />
      </ul>


      <button class="hide btn" @click="hideCompleted = !hideCompleted">
        {{ hideCompleted ? "Show all" : "Hide completed" }}
      </button>
      <button @click="deleteSelectedTodos" class="btn hide">Delete completed</button>
    </div>
  </div>

</template>

<style>
body {
  margin: 0;
  padding: 0;
  font-size: 18px;
  box-sizing: border-box;
  background: #eee;
}

.container {
  width: 70%;
  margin: auto;
  max-width: 700px;
}

* {
  font-family: "Quicksand";
}

ul {
  list-style-type: none;
}

.btn {
  border: none;
  outline: none;
  padding: .7rem 2rem;
  background: #999;
  color: white;
  text-transform: uppercase;
  margin: 1rem;
  transition: 0.2s ease;
}

.btn:hover {
  background: #bbb;
  cursor: pointer;
}

.todo-container {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.todo-list {
  width: 100%;
  padding: 0;
}

.hide {
  width: 100%;
  background: lightseagreen;
}
</style>

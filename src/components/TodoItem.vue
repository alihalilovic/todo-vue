<script setup>
defineProps(["todos"])

const emit = defineEmits(["todoClick", "todoDelete"])
</script>

<template>
  <TransitionGroup name="list">
    <li v-for="todo in todos" :key="todo.id" @click="emit('todoClick', todo)" class="todo"
      :class="{ striked: todo.checked }">
      <input type="checkbox" v-model="todo.checked">
      {{ todo.text }}
      <button class="delete" @click="emit('todoDelete', todo)">❌</button>
    </li>
  </TransitionGroup>
</template>

<style scoped>
.todo {
  padding: .5rem;
  border-bottom: 1px solid #ccc;
}

.todo:hover {
  cursor: pointer;
}

.striked {
  text-decoration: line-through;
}

.delete {
  border: none;
  outline: none;
  background: white;
  border-radius: 50%;
  padding: .5rem;
  transition: 0.4s ease;
}

.delete:hover {
  transform: rotate(45deg);
}

.list-enter-active,
.list-leave-active {
  transition: all 0.4s ease;
}

.list-enter-from {
  transform: translateX(-40px);
}
.list-leave-to {
  opacity: 0;
  transform: translateX(40px);
}
</style>
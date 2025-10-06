<script setup>
// Define the props this component expects
defineProps({
  todo: {
    type: Object,
    required: true
  }
})

const emit = defineEmits(['toggle-complete', 'remove-item'])

const toggle = (id) => {
  emit('toggle-complete', id)
}

const remove = (id) => {
  emit('remove-item', id)
}
</script>

<template>
  <li :class="{ 'todo-item-done': todo.completed }">
    <span @click="toggle(todo.id)">
      {{ todo.text }}
    </span>
    
    <button @click="remove(todo.id)">❌</button>
  </li>
</template>

<style scoped>
li {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 8px 0;
  border-bottom: 1px solid #eee;
}

li span {
  flex-grow: 1;
  cursor: pointer;
  padding: 5px;
}

.todo-item-done {
  text-decoration: line-through;
  color: #888;
}

button {
  background: none;
  border: none;
  cursor: pointer;
  font-size: 1.1em;
}
</style>
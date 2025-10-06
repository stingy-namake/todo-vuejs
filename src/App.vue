<script setup>
import { ref, computed } from 'vue'
import TodoItem from './components/TodoItem.vue'

const nextId = ref(3)
const newTodoText = ref('')
const todos = ref([
  { id: 1, text: 'Create ToDo List', completed: true },
  { id: 2, text: 'Send the homework', completed: false },
])


const addTodo = () => {
  const text = newTodoText.value.trim()

  if (text) {
    todos.value.push({
      id: nextId.value++,
      text: text,
      completed: false
    })
    newTodoText.value = ''
  }
}

const toggleCompletion = (id) => {
  const todo = todos.value.find(t => t.id === id)
  if (todo) {
    todo.completed = !todo.completed
  }
}

const removeItem = (id) => {
  todos.value = todos.value.filter(t => t.id !== id)
}


const remainingTasks = computed(() => {
  return todos.value.filter(t => !t.completed).length
})

</script>

<template>
  <div class="todo-app-container">
    <h1>To-Do List</h1>

    <div class="input-area">
      <input 
        type="text" 
        v-model="newTodoText" 
        placeholder="Add a new task..."
        @keyup.enter="addTodo" 
      />
      <button @click="addTodo" :disabled="!newTodoText.trim()">Add</button>
    </div>

    <ul class="todo-list">
      <TodoItem 
        v-for="todo in todos" 
        :key="todo.id" 
        :todo="todo" 
        @toggle-complete="toggleCompletion"
        @remove-item="removeItem"
      />
    </ul>

    <p v-if="todos.length === 0" class="empty-message">List is empty.</p>
    <p v-else class="status-message">
      You have {{ todos.length }} tasks in total. {{ remainingTasks }} remaining.
    </p>
  </div>
</template>

<style scoped>
.todo-app-container {
  max-width: 400px;
  margin: 50px auto;
  padding: 20px;
  border: 1px solid #ccc;
  border-radius: 8px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

h1 {
  text-align: center;
}

.input-area {
  display: flex;
  margin-bottom: 20px;
}

.input-area input {
  flex-grow: 1;
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 4px 0 0 4px;
}

.input-area button {
  padding: 10px 15px;
  border: none;
  background-color: #42b883;
  border-radius: 0 4px 4px 0;
  cursor: pointer;
}

.input-area button:disabled {
  background-color: #aae0c7;
  cursor: not-allowed;
}

.todo-list {
  list-style: none;
  padding: 0;
}

.status-message, .empty-message {
  text-align: center;
  margin-top: 20px;
  padding-top: 10px;
  border-top: 1px solid #eee;
}
</style>
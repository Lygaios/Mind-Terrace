<template>
  <div class="todo-list">
    <h3>To-Do List</h3>
    <div class="add-todo">
      <input 
        v-model="newTodo" 
        @keyup.enter="addTodo"
        placeholder="What needs to be done?"
        class="todo-input"
      />
      <button @click="addTodo" class="add-button">Add</button>
    </div>
    
    <ul class="todos" v-if="todos.length">
      <li v-for="todo in todos" :key="todo.id" class="todo-item">
        <input 
          type="checkbox" 
          v-model="todo.completed"
          class="todo-checkbox"
        />
        <span :class="{ completed: todo.completed }" class="todo-text">
          {{ todo.text }}
        </span>
        <button @click="removeTodo(todo.id)" class="remove-button">×</button>
      </li>
    </ul>
    
    <p v-else class="empty-state">No tasks yet. Add one above!</p>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'

// Define the Todo interface
interface Todo {
  id: number
  text: string
  completed: boolean
}

// Reactive state with proper typing
const todos = ref<Todo[]>([])
const newTodo = ref<string>('')

// Methods
const addTodo = (): void => {
  if (newTodo.value.trim()) {
    todos.value.push({
      id: Date.now(),
      text: newTodo.value.trim(),
      completed: false
    })
    newTodo.value = ''
  }
}

const removeTodo = (id: number): void => {
  todos.value = todos.value.filter(todo => todo.id !== id)
}
</script>

<style scoped>
.todo-list h3 {
  color: #2c3e50;
  margin-bottom: 1.5rem;
  font-weight: 300;
  font-size: 1.5rem;
}

.add-todo {
  display: flex;
  gap: 0.5rem;
  margin-bottom: 1.5rem;
}

.todo-input {
  flex: 1;
  padding: 0.8rem;
  border: none;
  border-radius: 10px;
  background: rgba(255, 255, 255, 0.2);
  color: #2c3e50;
  font-size: 1rem;
}

.todo-input::placeholder {
  color: rgba(44, 62, 80, 0.6);
}

.todo-input:focus {
  outline: none;
  background: rgba(255, 255, 255, 0.3);
}

.add-button, .remove-button {
  padding: 0.8rem 1.2rem;
  border: none;
  border-radius: 10px;
  background: rgba(52, 152, 219, 0.8);
  color: white;
  cursor: pointer;
  transition: background 0.3s ease;
}

.add-button:hover {
  background: rgba(52, 152, 219, 1);
}

.remove-button {
  background: rgba(231, 76, 60, 0.8);
  padding: 0.4rem 0.8rem;
  font-size: 1.2rem;
  line-height: 1;
}

.remove-button:hover {
  background: rgba(231, 76, 60, 1);
}

.todos {
  list-style: none;
}

.todo-item {
  display: flex;
  align-items: center;
  gap: 0.8rem;
  padding: 0.8rem;
  margin-bottom: 0.5rem;
  background: rgba(255, 255, 255, 0.1);
  border-radius: 10px;
  transition: background 0.3s ease;
}

.todo-item:hover {
  background: rgba(255, 255, 255, 0.2);
}

.todo-checkbox {
  width: 1.2rem;
  height: 1.2rem;
}

.todo-text {
  flex: 1;
  color: #2c3e50;
}

.todo-text.completed {
  text-decoration: line-through;
  opacity: 0.6;
}

.empty-state {
  text-align: center;
  color: rgba(44, 62, 80, 0.6);
  font-style: italic;
  padding: 2rem;
}
</style>
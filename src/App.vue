<template>
  <div id="app">
    <h1>Todo List</h1>
    <div class="input-container">
      <input v-model="newTodo" @keyup.enter="addTodo" placeholder="Add a new task"/>
      <button @click="addTodo">Add</button>
    </div>
    <ul>
      <li v-for="(todo, index) in todos" :key="index" class="todo-item">
        <input type="checkbox" v-model="todo.done">
        <span :class="{ done: todo.done }">{{ todo.text }}</span>
        <button @click="removeTodo(index)">Delete</button>
      </li>
    </ul>
    <p>Incomplete tasks: {{ incompleteTodos }}</p>
  </div>
</template>

<script>
import { ref, computed } from 'vue';
import { useTodoStore } from './stores/todoStore';

export default {
  setup() {
    const newTodo = ref('');
    const todoStore = useTodoStore();

    const addTodo = () => {
      if (newTodo.value.trim()) {
        todoStore.addTodo(newTodo.value);
        newTodo.value = '';
      }
    };

    const removeTodo = (index) => {
      todoStore.removeTodo(index);
    };

    const incompleteTodos = computed(() => todoStore.incompleteTodos);

    return {
      newTodo,
      todos: todoStore.todos,
      addTodo,
      removeTodo,
      incompleteTodos
    };
  }
};
</script>

<style>
body {
  font-family: 'Times New Roman', Times, serif;
  background: linear-gradient(135deg, #eceff1, #ffffff);
  margin: 0;
  padding: 0;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
}

#app {
  background-color: #ffffff;
  padding: 30px;
  border-radius: 15px;
  box-shadow: 0 10px 20px rgba(0, 0, 0, 0.1);
  width: 100%;
  max-width: 400px;
  transition: transform 0.3s;
}

#app:hover {
  transform: translateY(-5px);
}

h1 {
  margin-bottom: 20px;
  font-size: 26px;
  text-align: center;
  color: #333;
  font-weight: bold;
}

.input-container {
  display: flex;
  margin-bottom: 20px;
}

input[type="text"] {
  flex: 1;
  padding: 12px;
  font-size: 16px;
  border: 1px solid #ddd;
  border-radius: 8px 0 0 8px;
  box-shadow: inset 0 2px 4px rgba(0, 0, 0, 0.1);
  transition: border-color 0.3s, box-shadow 0.3s;
}

input[type="text"]:focus {
  border-color: #66afe9;
  box-shadow: 0 0 8px rgba(102, 175, 233, 0.5);
  outline: none;
}

button {
  padding: 12px 20px;
  font-size: 16px;
  border: none;
  background-color: #28a745;
  color: #fff;
  cursor: pointer;
  border-radius: 0 8px 8px 0;
  transition: background-color 0.3s, box-shadow 0.3s;
}

button:hover {
  background-color: #218838;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
}

ul {
  list-style-type: none;
  padding: 0;
  margin: 0;
}

.todo-item {
  display: flex;
  align-items: center;
  padding: 12px;
  border-bottom: 1px solid #ddd;
  transition: background-color 0.3s, transform 0.3s;
}

.todo-item:hover {
  background-color: #f1f1f1;
  transform: translateX(10px);
}

.todo-item:last-child {
  border-bottom: none;
}

.todo-item input[type="checkbox"] {
  margin-right: 12px;
  transform: scale(1.2);
  cursor: pointer;
  transition: transform 0.3s;
}

.todo-item input[type="checkbox"]:hover {
  transform: scale(1.3);
}

.todo-item span {
  flex: 1;
  font-size: 16px;
}

.todo-item button {
  padding: 6px 12px;
  font-size: 14px;
  border: none;
  background-color: #dc3545;
  color: #fff;
  cursor: pointer;
  border-radius: 4px;
  transition: background-color 0.3s, transform 0.3s;
}

.todo-item button:hover {
  background-color: #c82333;
  transform: scale(1.1);
}

.done {
  text-decoration: line-through;
  color: #888;
}
</style>

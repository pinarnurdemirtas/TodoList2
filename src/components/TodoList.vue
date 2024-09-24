<template>
    <div class="todo-app">
      <h1>Todo List</h1>
      <form @submit.prevent="addTodo">
        <input v-model="newTodo" placeholder="Yeni görev ekle..." />
        <button type="submit">Ekle</button>
      </form>
      <ul>
        <li v-for="todo in todos" :key="todo.id" :class="{ completed: todo.completed }">
          <input type="checkbox" v-model="todo.completed" />
          <span @dblclick="editTodo(todo)" :contenteditable="todo.editing" @blur="saveEdit(todo, $event)">
            {{ todo.text }}
          </span>
          <button @click="removeTodo(todo.id)">Sil</button>
        </li>
      </ul>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        newTodo: '',
        todos: [],
        nextId: 1,
      };
    },
    methods: {
      addTodo() {
        if (this.newTodo.trim()) {
          this.todos.push({
            id: this.nextId++,
            text: this.newTodo.trim(),
            completed: false,
            editing: false,
          });
          this.newTodo = '';
        }
      },
      removeTodo(id) {
        this.todos = this.todos.filter(todo => todo.id !== id);
      },
      editTodo(todo) {
        todo.editing = true;
      },
      saveEdit(todo, event) {
        const newText = event.target.innerText.trim();
        if (newText) {
          todo.text = newText;
        } else {
          this.removeTodo(todo.id);
        }
        todo.editing = false;
      },
    },
  };
  </script>
  
  <!-- SCSS dosyasını import ediyoruz -->
  <style lang="scss" scoped>
  @import '../styles/TodoList.scss';
  </style>
  
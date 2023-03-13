<template>
  <div class="wrapper">
    <div class="left-box">
      <h2>Todo List</h2>
      <ul>
        <li v-for="todo in todos" :key="todo.id" class="list">
          <input
            type="checkbox"
            v-model="todo.completed"
            @change="updateTodo(todo)"
          />
          {{ todo.title }}
          <button @click="deleteTodo(todo)" class="delete-btn">Delete</button>
        </li>
      </ul>
      <form @submit.prevent="addTodo" class="forms">
        <input
          type="text"
          v-model="newTodo"
          placeholder="Add new todo..."
          class="myInput"
        />
        <button type="submit" class="add-btn">Add</button>
      </form>
    </div>
    <div class="right-box"></div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      todos: [],
      newTodo: "",
    };
  },
  created() {
    this.fetchTodos();
  },
  methods: {
    fetchTodos() {
      const todos = JSON.parse(localStorage.getItem("todos")) || [];
      this.todos = todos;
    },
    saveTodos() {
      localStorage.setItem("todos", JSON.stringify(this.todos));
    },
    addTodo() {
      if (this.newTodo.trim()) {
        const newTodo = {
          id: Date.now(),
          title: this.newTodo,
          completed: false,
        };
        this.todos.push(newTodo);
        this.saveTodos();
        this.newTodo = "";
      }
    },
    updateTodo(todo) {
      axios
        .put(`https://jsonplaceholder.typicode.com/todos/${todo.id}`, todo)
        .then(() => {
          this.saveTodos();
        })
        .catch((error) => {
          console.error(error);
        });
    },
    deleteTodo(todo) {
      axios
        .delete(`https://jsonplaceholder.typicode.com/todos/${todo.id}`)
        .then(() => {
          this.todos.splice(this.todos.indexOf(todo), 1);
          this.saveTodos();
        })
        .catch((error) => {
          console.error(error);
        });
    },
  },
};
</script>

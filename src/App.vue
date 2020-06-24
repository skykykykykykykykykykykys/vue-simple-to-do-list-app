<template>
  <div id="app" class='container'>
    <img src="./assets/logo.png" class="logo">

    <todo-list></todo-list>
  </div>
</template>

<script>
//import Header from './components/layout/Header';
//import Todos from './components/Todos';
//import AddTodo from './components/AddTodo';

import TodoList from './components/TodoList';
//import axios from 'axios';


export default {
  name: 'App',
  components: {
    TodoList
  },
  /*
  data() {
    return {
        todos: []
      }
  },
  methods: {
    deleteTodo(id) {
      this.todos = this.todos.filter(todo => todo.id !== id);
    },
    addTodo(newTodo) {
      const { title, completed } = newTodo;

      axios.post('https://jsonplaceholder.typicode.com/todos', {
        title,
        completed
      })
        .then(res => this.todos = [...this.todos, res.data])
        .catch(err => console.log(err)); 
      console.log("adding Todo success")
    }
  },
  created() {
    axios.get('https://jsonplaceholder.typicode.com/todos?_limit=10')
      .then(res => this.todos = res.data)
      .catch(err => console.log(err));
  }
  */
}
</script>

<style>

* {
  box-sizing: border-box;
}
.container {
  max-width: 600px;
  margin: 0 auto;
}
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  /* text-align: center; */
  color: #2c3e50;
  margin-top: 60px;
  font-size: 24px;
}
.logo {
  display: block;
  margin: 20px auto;
  height: 75px;
}
</style>

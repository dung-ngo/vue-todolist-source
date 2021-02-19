<template>
  <div id="app">
    <!-- @ = v-on | : = v-bind -->
    <AddTodo @add-todo="addTodo"/>
    <Todos v-bind:todos="todos" v-on:del-todo="deleteTodo"/>
  </div>
</template>

<script>
import Todos from '../components/Todos';
import AddTodo from '../components/AddTodo';
import axios from 'axios';


export default {
  name: 'Home',
  components: {
    Todos,
    AddTodo
  },
  data() {
    return {
      todos: []
    }
  },
  methods: {
    deleteTodo(id) {
      axios.delete(`https://jsonplaceholder.typicode.com/todos/${id}`)
        .then(() => this.todos = this.todos.filter(todo => todo.id != id))
        .catch(err => console.log(err));
    },
    addTodo(newTodo){
      const {id, title, completed} = newTodo;

      axios.post('https://jsonplaceholder.typicode.com/todos', {
        id,
        title,
        completed
      })
        .then(() => this.todos = [...this.todos, newTodo])
        .catch(err => console.log(err));
    }
  },
  created(){
    axios.get('https://jsonplaceholder.typicode.com/todos?_limit=5')
      .then(res => this.todos = res.data)
      .catch(err => console.log(err));
  }
}
</script>

<style>
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
 }

body {
  font-family: Arial, Arial, Helvetica, sans-serif;
  line-height: 1.4;
}
</style>

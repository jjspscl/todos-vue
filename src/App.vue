<template>
  <div id="app">
    <Header />
    <AddTodo
      v-on:add-todo="addTodo" />
    <Todos v-bind:todos="todos"
          v-on:del-todo="deleteTodo" />
  </div>
</template>

<script>
import Header from './components/layout/Header'
import Todos from './components/Todos';
import AddTodo from './components/AddTodo';
import axios from 'axios';
export default {
  name: 'App',
  components: {
    Header, Todos, AddTodo
  },
  data() {
    return {
      todos: []
    }
  },
  methods:{
    deleteTodo(id){
      axios.delete(`http://localhost:8000/api/todos/${id}`)
        // .then(res =>)
        .then( this.todos = this.todos
          .filter(todo => todo.id !== id))
        .catch(err => console.log(err))
      this.todos = this.todos.filter(todo => todo.id !== id);
    },
    addTodo(newTodo){
      const {title,completed} = newTodo;
      axios.post('http://localhost:8000/api/todos/',{
        title,
        completed
      })
        .then(res => this.todos = [...this.todos, res.data])
        .catch(err => console.log(err));
    }
  },
  created() {
    axios.get("http://localhost:8000/api/todos/") //_limit=5
      .then(res => {
        this.todos = res.data
        console.log(res.data)
      })
      .catch(err => console.log(err))
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body{
  line-height: 1.4;
}

.btn{
  display: inline-block;
  border: none;
  background: #555;
  color: #fff;
  padding: 7px 20px;
  cursor: pointer;
}

.btn:hover{
  background: #666;
}
</style>

<template>
  <div id="app">
    <Header />
    <AddTodo v-on:add-todo="addTodo" />
    <Todos v-bind:todos="todos" v-on:del-todo="deleteTodo" />
  </div>
</template>

<script>
import Header from './components/layout/Header';
import Todos from './components/Todos';
import AddTodo from './components/AddTodo';

export default {
  name: 'app',
  components: {
    Header,
    Todos,
    AddTodo
  },
  data() {
    return {
      todos: []
    };
  },
  methods: {
    async deleteTodo(id) {
      const options = {
        method: 'DELETE',
        headers: {
          'Content-Type': 'application/json'
        }
      };
      const response = await fetch(
        `https://jsonplaceholder.typicode.com/todos/${id}`,
        options
      );

      if (!response.ok) {
        throw new Error(`Could not delete todo, please try again later.`);
      }

      this.todos = this.todos.filter(todo => todo.id !== id);
    },
    async addTodo(newTodo) {
      const options = {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify(newTodo)
      };
      const response = await fetch(
        'https://jsonplaceholder.typicode.com/todos',
        options
      );
      if (!response.ok) {
        throw new Error(`Could not add new todo, please try again later.`);
      }
      const data = await response.json();

      this.todos = [...this.todos, data];
    }
  },
  async mounted() {
    const response = await fetch(
      'https://jsonplaceholder.typicode.com/todos?_limit=15'
    );
    if (!response.ok) {
      throw new Error(`Could not get todos, please try again later.`);
    }
    const data = await response.json();
    this.todos = data;
  }
};
</script>

<style>
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  font-family: Arial, Helvetica, sans-serif;
  line-height: 1.4;
}

.btn {
  display: inline-block;
  border: none;
  background: #555;
  color: #fff;
  padding: 7px 20px;
  cursor: pointer;
}

.btn:hover {
  background: #666;
}
</style>

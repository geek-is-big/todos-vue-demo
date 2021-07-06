<template>
  <div>
    <h2>Todo application</h2>
    <router-link to="/">Home</router-link>
    <div style="display: flex">
      <AddTodo @add-todo="addTodo" />
      <select v-model="filter" style="margin: 0 20px">
        <option value="all">All</option>
        <option value="completed">Completed</option>
        <option value="not-completed">Not completed</option>
      </select>
    </div>
    <hr />
    <Loader v-if="loading" />
    <TodoList v-else-if="filteredTodos.length" v-bind:todos="filteredTodos" @remove-todo="removeTodo" />
    <p v-else>No todos!</p>
  </div>
</template>

<script>
import TodoList from '@/components/TodoList'
import AddTodo from '@/components/AddTodo'
import Loader from '@/components/Loader'
export default {
  name: 'App',
  data() {
    return {
      todos: [],
      loading: true,
      filter: 'all',
    }
  },
  mounted() {
    fetch('https://jsonplaceholder.typicode.com/todos/?_limit=3')
      .then((response) => response.json())
      .then((json) => {
        setTimeout(() => {
          this.todos = json
          this.loading = false
        }, 200)
      })
  },
  // watch: {
  //   filter(value) {
  //     console.log(value)
  //   }
  // },
  computed: {
    filteredTodos() {
      if (this.filter === 'completed') {
        return this.todos.filter((t) => t.completed)
      } else if (this.filter === 'not-completed') {
        return this.todos.filter((t) => !t.completed)
      } else {
        return this.todos
      }
    },
  },
  components: {
    TodoList,
    AddTodo,
    Loader,
  },

  methods: {
    removeTodo(id) {
      this.todos = this.todos.filter((t) => t.id !== id)
    },
    addTodo(todo) {
      this.todos.push(todo)
    },
  },
}
</script>

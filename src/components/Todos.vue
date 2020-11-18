<template>
  <div class="todos-container">
      <TodosItem v-for="todo in todos" :key="todo.id" v-bind:todo="todo"
      @toggle-event="toggleCompleted($event)"></TodosItem>
  </div>
</template>

<script>
import axios from "axios";
import TodosItem from "./TodosItem"

export default {
  name: 'Todos',
  components: {
    TodosItem
  },
  props: {
  },
  data() {
      return {
        todosApi: 'https://jsonplaceholder.typicode.com/todos',
          todos: []
      }
  },
  created() {
    this.getTodos();
  },
  methods: {
      getTodos() {
              axios.get(`${this.todosApi}?_limit=5`).then(res => {
                if (res.data) {
                    this.todos = res.data;
                }
              }).catch(err => console.log(err));
      },
      toggleCompleted(todo) {
          console.log(todo)
          axios.put(`${this.todosApi}/${todo.id}`).then(res => {
              const { data: {id} } = res;
              const foundTodo = this.todos.findIndex(item => item.id == id);
              if (foundTodo > -1) {
                  this.todos[foundTodo].completed = !this.todos[foundTodo].completed;
              }
          }).catch(err => console.log(err))
      },
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.todos-item {
    box-shadow: 0 4px 8px 0 rgba(0,0,0,0.2);
  transition: 0.3s;
  border-radius: 8px;
  margin: 20px;
  padding:20px;
  background: orange;
}
.completed {
    background-color: green;
}
</style>

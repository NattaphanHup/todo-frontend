<template>
  <div id="app">
    <h1>Todo List</h1>
    <input
      v-model="newTodo"
      @keyup.enter="addTodo"
      placeholder="Add new todo"
    />
    <button @click="addTodo">Add</button>
    <ul>
      <li v-for="todo in todos" :key="todo.id">
        <span
          :class="{ completed: todo.completed }"
          style="margin-right: 0.6rem"
          >{{ todo.text }}</span
        >
        <button @click="toggleCompleted(todo)">Toggle</button>
        <button @click="deleteTodo(todo.id)">Delete</button>
      </li>
    </ul>
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
  async created() {
    this.fetchTodos();
  },
  methods: {
    async fetchTodos() {
      const res = await axios.get(`${process.env.VUE_APP_API_URL}/todos`); // เปลี่ยนเป็น backend URL จริงตอน deploy
      this.todos = res.data;
    },
    async addTodo() {
      if (this.newTodo) {
        await axios.post(`${process.env.VUE_APP_API_URL}/todos`, {
          text: this.newTodo,
        });
        this.newTodo = "";
        this.fetchTodos();
      }
    },
    async toggleCompleted(todo) {
      console.log(todo);
      const res = await axios.put(
        `${process.env.VUE_APP_API_URL}/todos/${todo.id}`,
        {
          completed: !todo.completed,
        }
      );
      this.fetchTodos();
    },
    async deleteTodo(id) {
      await axios.delete(`${process.env.VUE_APP_API_URL}/todos/${id}`);
      this.fetchTodos();
    },
  },
};
</script>

<style>
.completed {
  text-decoration: line-through;
}
#app {
  justify-items: center;
}
</style>

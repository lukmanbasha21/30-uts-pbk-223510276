<template>
  <div class="container">
    <h1>CLUB BOLA</h1>
    <input
      type="text"
      v-model="newTodo"
      @keyup.enter="addTodo"
      placeholder="Masukkan Nama Club"
    />
    <ul>
      <li v-for="(todo, index) in filteredTodos" :key="index">
        <input type="checkbox" v-model="todo.completed" />
        <span :class="{ completed: todo.completed }">{{ todo.text }}</span>
        <button @click="removeTodo(index)">Hapus</button>
      </li>
    </ul>
    <p v-if="remainingCount === 0">Ayo Bertanding.</p>
    <p v-else>{{ remainingCount }} task(s) remaining</p>
    <button @click="toggleFilter">{{ filterButtonText }}</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newTodo: "",
      todos: [],
      showCompleted: false,
    };
  },
  computed: {
    filteredTodos() {
      return this.showCompleted
        ? this.todos
        : this.todos.filter((todo) => !todo.completed);
    },
    remainingCount() {
      return this.todos.filter((todo) => !todo.completed).length;
    },
    filterButtonText() {
      return this.showCompleted ? "Show Uncompleted" : "Show All";
    },
  },
  methods: {
    addTodo() {
      if (this.newTodo.trim() !== "") {
        this.todos.push({ text: this.newTodo, completed: false });
        this.newTodo = "";
      }
    },
    removeTodo(index) {
      this.todos.splice(index, 1);
    },
    toggleFilter() {
      this.showCompleted = !this.showCompleted;
    },
  },
};
</script>

<style>
body {
  background-image: url("./assets/bg.jpeg");
}
.container {
  margin: 0 auto;
  padding: 25px;
  font-family: Arial, sans-serif;
  background-image: url("./assets/bg1.jpeg");
  border-radius: 10px;
  width: 500px 400px;
}

.completed {
  text-decoration: line-through;
}
</style>

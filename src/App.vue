<template>
  <div>
    <header class="navbar">
      <nav>
        <ul>
          <li @click="showTodos">Todos</li>
          <li @click="showPosts">Post</li>
        </ul>
      </nav>
    </header>
    <div class="container">
      <h1 v-if="activeTab === 'todos'">CLUB BOLA</h1>
      <h1 v-else-if="activeTab === 'posts'">Postingan User</h1>
      <div v-if="activeTab === 'todos'">
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
      <div v-else-if="activeTab === 'posts'">
        <select v-model="selectedUser">
          <option v-for="user in users" :key="user.id" :value="user.id">
            {{ user.name }}
          </option>
        </select>
        <div v-if="posts.length">
          <div v-for="post in filteredPosts" :key="post.id">
            <h2>{{ post.title }}</h2>
            <p>{{ post.body }}</p>
          </div>
        </div>
        <p v-else>Loading...</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newTodo: "",
      todos: [],
      showCompleted: false,
      activeTab: "todos",
      users: [],
      posts: [],
      selectedUser: null,
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
    filteredPosts() {
      return this.posts.filter(
        (post) => post.userId === parseInt(this.selectedUser)
      );
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
    async fetchUsers() {
      try {
        const response = await fetch(
          "https://jsonplaceholder.typicode.com/users"
        );
        this.users = await response.json();
      } catch (error) {
        console.error("Error fetching users:", error);
      }
    },
    async fetchPosts() {
      try {
        const response = await fetch(
          "https://jsonplaceholder.typicode.com/posts"
        );
        this.posts = await response.json();
      } catch (error) {
        console.error("Error fetching posts:", error);
      }
    },
    async showPosts() {
      this.activeTab = "posts";
      if (!this.users.length) {
        await this.fetchUsers();
      }
      if (!this.posts.length) {
        await this.fetchPosts();
      }
    },
    showTodos() {
      this.activeTab = "todos";
    },
  },
  mounted() {
    this.fetchUsers();
  },
};
</script>

<style>
body {
  background-image: url("./src/assets/bg.jpeg");
  background-repeat: no-repeat;
  background-size: cover;
}

.container {
  margin: 0 auto;
  padding: 45px;
  font-family: Arial, sans-serif;
  background-image: url("./src/assets/bg1.jpeg");
  background-size: cover;
  border-radius: 10px;
  width: 500px 400px;
}

.completed {
  text-decoration: line-through;
}

.navbar {
  background-color: rgb(26, 21, 86);
  overflow: hidden;
  display: flex; /* Tambahkan */
  justify-content: center; /* Tambahkan */
}

.navbar ul {
  list-style-type: none;
  margin: 0;
  padding: 0;
}

.navbar ul li {
  float: left;
}
</style>

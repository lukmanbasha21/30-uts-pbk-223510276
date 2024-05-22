<template>
  <div>
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
</template>

<script>
export default {
  props: {
    users: Array,
    posts: Array,
  },
  data() {
    return {
      selectedUser: null,
    };
  },
  computed: {
    filteredPosts() {
      return this.selectedUser
        ? this.posts.filter((post) => post.userId === this.selectedUser)
        : this.posts;
    },
  },
};
</script>

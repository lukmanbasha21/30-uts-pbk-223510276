<template>
  <div class="q-pa-md">
    <q-page padding>
      <q-card class="q-ma-md">
        <q-card-section>
          <h1>Posts</h1>
          <q-btn-dropdown
            split
            class="glossy"
            color="teal"
            :label="selectedUserLabel"
            @click="clearSelection"
          >
            <q-list>
              <q-item
                v-for="user in users"
                :key="user.id"
                clickable
                v-close-popup
                @click="onItemClick(user)"
              >
                <q-item-section avatar>
                  <q-avatar icon="person" color="primary" text-color="white" />
                </q-item-section>
                <q-item-section>
                  <q-item-label>{{ user.name }}</q-item-label>
                  <q-item-label caption>{{ user.email }}</q-item-label>
                </q-item-section>
              </q-item>
              <q-item clickable v-close-popup @click="onItemClick(null)">
                <q-item-section avatar>
                  <q-avatar icon="clear" color="negative" text-color="white" />
                </q-item-section>
                <q-item-section>
                  <q-item-label>All Users</q-item-label>
                  <q-item-label caption>Clear selection</q-item-label>
                </q-item-section>
              </q-item>
            </q-list>
          </q-btn-dropdown>
        </q-card-section>
        <q-card-section v-if="loading">
          Loading...
        </q-card-section>
        <q-card-section v-else>
          <q-list>
            <q-item v-for="post in filteredPosts" :key="post.id">
              <q-item-section>
                <q-item-label>
                  <h2>{{ post.title }}</h2>
                </q-item-label>
                <q-item-label caption>{{ post.body }}</q-item-label>
                <q-item-label caption>Author: {{ getUser(post.userId) }}</q-item-label>
              </q-item-section>
            </q-item>
          </q-list>
        </q-card-section>
      </q-card>
    </q-page>
  </div>
</template>
<script>
import { ref, computed, onMounted } from 'vue';
import { useQuasar } from 'quasar';

export default {
  setup() {
    const $q = useQuasar();
    const posts = ref([]);
    const users = ref([]);
    const selectedUserId = ref(null); // Null untuk semua pengguna
    const loading = ref(true);

    const fetchData = async () => {
      try {
        const [postsResponse, usersResponse] = await Promise.all([
          fetch("https://jsonplaceholder.typicode.com/posts"),
          fetch("https://jsonplaceholder.typicode.com/users"),
        ]);
        posts.value = await postsResponse.json();
        users.value = await usersResponse.json();
      } catch (error) {
        $q.notify({ type: 'negative', message: 'Error fetching data' });
        console.error("Error fetching data:", error);
      } finally {
        loading.value = false;
      }
    };

    const getUser = (userId) => {
      const user = users.value.find(user => user.id === userId);
      return user ? user.name : "Unknown";
    };

    const onItemClick = (user) => {
      selectedUserId.value = user ? user.id : null;
    };

    const clearSelection = () => {
      selectedUserId.value = null;
    };

    onMounted(fetchData);

    return {
      posts,
      users,
      selectedUserId,
      loading,
      getUser,
      filteredPosts: computed(() => {
        if (!selectedUserId.value) return posts.value;
        return posts.value.filter(post => post.userId === selectedUserId.value);
      }),
      selectedUserLabel: computed(() => {
        const user = users.value.find(user => user.id === selectedUserId.value);
        return user ? user.name : "All Users";
      }),
      onItemClick,
      clearSelection,
    };
  },
};
</script>
<style scoped>
h1 {
  margin-bottom: 20px;
}
h2 {
  margin: 0;
}
.q-item {
  margin-bottom: 20px;
}
.q-btn {
  width: 200px;
  text-align: left;
}
.q-chip {
  margin-top: 10px;
}
.q-avatar {
  margin-right: 10px;
}
</style>


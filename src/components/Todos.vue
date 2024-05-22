<template>
  <div>
    <input
      type="text"
      :value="newTodo"
      @input="$emit('updateNewTodo', $event.target.value)"
      @keyup.enter="$emit('addTodo')"
      placeholder="Masukkan Nama Club"
    />
    <ul>
      <li v-for="(todo, index) in todos" :key="index">
        <input type="checkbox" v-model="todo.completed" />
        <span :class="{ completed: todo.completed }">{{ todo.text }}</span>
        <button @click="$emit('removeTodo', index)">Hapus</button>
      </li>
    </ul>
    <p v-if="remainingCount === 0">Ayo Bertanding.</p>
    <p v-else>{{ remainingCount }} task(s) remaining</p>
    <button @click="$emit('toggleFilter')">{{ filterButtonText }}</button>
  </div>
</template>

<script>
export default {
  props: {
    todos: Array,
    newTodo: String,
  },
  computed: {
    remainingCount() {
      return this.todos.filter((todo) => !todo.completed).length;
    },
    filterButtonText() {
      return "Filter"; // or whatever logic you use to determine button text
    },
  },
};
</script>

<style scoped>
.completed {
  text-decoration: line-through;
}
</style>

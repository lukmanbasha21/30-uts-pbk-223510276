<template>
  <header class="navbar">
    <nav>
      <ul>
        <li @click="$emit('showTodos')">Todos</li>
        <li @click="$emit('showPosts')">Post</li>
      </ul>
    </nav>
  </header>
</template>

<script>
export default {};
</script>

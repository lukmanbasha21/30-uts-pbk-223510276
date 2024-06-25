<template>
  <q-page class="flex flex-center">
    <q-card class="q-pa-md q-my-lg" style="max-width: 400px; width: 100%;">
      <q-card-section>
        <h4>List Club Bola</h4>
        <q-input
          v-model="newTodo"
          @keyup.enter="addTodo"
          placeholder="Masukkan Nama Club"
          outlined
          class="q-my-md"
        >
          <template v-slot:append>
            <q-btn @click="addTodo" label="Add" color="primary" />
          </template>
        </q-input>
        <q-list bordered class="q-my-md">
          <q-item v-for="(todo, index) in filteredTodos" :key="index" clickable>
            <q-item-section>
              <q-checkbox v-model="todo.completed" />
            </q-item-section>
            <q-item-section>
              <span :class="{ completed: todo.completed }">{{ todo.text }}</span>
            </q-item-section>
            <q-item-section side>
              <q-btn @click="removeTodo(index)" color="negative" flat icon="delete" />
            </q-item-section>
          </q-item>
        </q-list>
        <q-card-actions align="around">
          <p>Total todos: {{ totalTodos }}</p>
          <q-btn @click="toggleFilter" :label="filterButtonText" color="secondary" />
        </q-card-actions>
      </q-card-section>
    </q-card>
  </q-page>
</template>

<script>
import { ref, computed } from "vue";
import { useTodoStore } from "../store/todoStore";
import { QPage, QCard, QCardSection, QInput, QBtn, QList, QItem, QItemSection, QCheckbox, QCardActions } from "quasar";

export default {
  components: {
    QPage,
    QCard,
    QCardSection,
    QInput,
    QBtn,
    QList,
    QItem,
    QItemSection,
    QCheckbox,
    QCardActions,
  },
  setup() {
    const newTodo = ref("");
    const todoStore = useTodoStore();

    const addTodo = () => {
      if (newTodo.value.trim() !== "") {
        todoStore.addTodo(newTodo.value);
        newTodo.value = "";
      }
    };

    const removeTodo = (index) => {
      todoStore.removeTodo(index);
    };

    const toggleFilter = () => {
      todoStore.toggleFilter();
    };

    return {
      newTodo,
      addTodo,
      removeTodo,
      toggleFilter,
      remainingCount: computed(() => todoStore.remainingCount),
      filteredTodos: computed(() => todoStore.filteredTodos),
      filterButtonText: computed(() => todoStore.filterButtonText),
      totalTodos: computed(() => todoStore.totalTodos), // New computed property for total todos
    };
  },
};
</script>

<style scoped>
.completed {
  text-decoration: line-through;
}
</style>

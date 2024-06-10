<template>
  <div>
    <header class="navbar">
      <nav>
        <ul>
          <li @click="showTodos">Todo List</li>
        </ul>
      </nav>
    </header>
    <div class="container">
      <h1 v-if="activeTab === 'todos'">RUN COMPETITION</h1>
      <div v-if="activeTab === 'todos'">
        <input
          type="text"
          v-model="newTodo"
          @keyup.enter="addTodo"
          placeholder="Masukkan Nama"
        />
        <ul>
          <li
            v-for="(todo, index) in filteredTodos(showCompleted)"
            :key="index"
          >
            <input type="checkbox" v-model="todo.completed" />
            <span :class="{ completed: todo.completed }">{{ todo.text }}</span>
            <button @click="removeTodo(index)">Hapus</button>
          </li>
        </ul>
        <p v-if="remainingCount === 0">Lets Run!!!</p>
        <p v-else>{{ remainingCount }} task(s) remaining</p>
        <button @click="toggleFilter">{{ filterButtonText }}</button>
      </div>
    </div>
  </div>
</template>

<script>
import { ref, computed } from "vue";
import { useTodosStore } from "./store/todos";
import { defineComponent } from "vue";

export default defineComponent({
  setup() {
    const todosStore = useTodosStore();
    const newTodo = ref("");
    const showCompleted = ref(false);
    const activeTab = ref("todos");

    const addTodo = () => {
      if (newTodo.value.trim() !== "") {
        todosStore.addTodo(newTodo.value);
        newTodo.value = "";
      }
    };

    const removeTodo = (index) => {
      todosStore.removeTodo(index);
    };

    const toggleFilter = () => {
      showCompleted.value = !showCompleted.value;
    };

    const filterButtonText = computed(() => {
      return showCompleted.value ? "Show Uncompleted" : "Show All";
    });

    return {
      newTodo,
      showCompleted,
      activeTab,
      addTodo,
      removeTodo,
      toggleFilter,
      filterButtonText,
      remainingCount: computed(() => todosStore.remainingCount),
      filteredTodos: todosStore.filteredTodos,
      showTodos: () => {
        activeTab.value = "todos";
      },
    };
  },
});
</script>

<style>
.completed {
  text-decoration: line-through;
}

.navbar {
  background-color: rgb(237, 226, 12);
  overflow: hidden;
  display: flex;
  justify-content: center;
}

.navbar ul {
  list-style-type: none;
  margin: 0;
  padding: 0;
}

.navbar ul li {
  float: left;
  cursor: pointer;
  padding: 14px 16px;
  color: rgb(195, 195, 11);
}

.navbar ul li:hover {
  background-color: rgb(64, 51, 2);
}

.container {
  max-width: 800px;  /* Lebar maksimum dari container */
  margin: 0 auto;    /* Mengatur margin secara otomatis untuk memusatkan */
  padding: 20px;     /* Memberikan ruang di dalam container */
  display: block;    /* Pastikan display-nya block agar margin bekerja */
  align-items: center;
  justify-content: space-between;
}


input[type="text"] {
  width: 100%;
  padding: 10px;
  margin: 10px 0;
  box-sizing: border-box;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 10px;
  background: #f9f9f9; 
  margin: 5px 0;
  border: 1px solid #ddd;
}

li button {
  background: rgb(72, 86, 2);
  color: white;
  border: none;
  padding: 5px 10px;
  cursor: pointer;
}

li button:hover {
  background: rgba(83, 99, 6, 0.522);
}
</style>

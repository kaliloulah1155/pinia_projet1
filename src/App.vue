<template>
  <main>
    <!-- Heading-->
    <header>
      <img src="./assets/pinia-logo.png" alt="pinia-logo" />
      <h1>Pinia Tasks</h1>
    </header>
    <!-- Task input -->
    <div class="new-task-form">
      <TaskForm />
    </div>
    <!-- Task button-->
    <nav class="filter">
      <button @click="btnAll">All tasks</button>
      <button @click="btnFavs">Favs tasks</button>
      <button @click="taskStore.$reset">Reset state</button>
    </nav>
    <!-- loading -->
    <div v-if="loading" class="loading">Loadin tasks...</div>
    
    <!-- Task list -->
    <div class="task-list" v-if="filter === 'all'">
      <p>You have ({{ totalCount }}) tasks left to do</p>
      <div v-for="task in tasks" :key="task.id">
        <task-details :task="task" />
      </div>
    </div>
    <div class="task-list" v-if="filter === 'favs'">
      <p>You have ({{ favCount }}) favs left to do</p>
      <div v-for="task in favs" :key="task.id">
        <task-details :task="task" />
      </div>
    </div>
  
  

  </main>
</template>

<script>
import { storeToRefs } from "pinia";
import { ref } from "vue";
import TaskDetails from "./components/TaskDetails.vue";
import { useTaskStore } from "./stores/TaskStore";
import TaskForm from './components/TaskForm.vue';

export default {
  components: { TaskDetails,TaskForm },
  setup() {
    const taskStore = useTaskStore();
    const filter = ref("all");
    
    const {
      tasks,
      loading,
      favs,
      totalCount,
      favCount
    } = storeToRefs(taskStore)

    //fetch tasks 
     taskStore.getTasks()
    return { 
      taskStore,
      filter,
      tasks,
      loading,
      favs,
      totalCount,
      favCount 
    };
  },
  methods: {
    btnFavs() {
      this.filter = "favs";
    },
    btnAll() {
      this.filter = "all";
    },
  },
};
</script>

<style lang="css" scoped>
@import "./assets/main.css"; 
</style>

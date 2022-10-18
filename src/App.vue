<template>
  <main>
    <!-- heading -->
    <header>
      <img src="./assets/logo.svg" alt="pinia logo">
      <h1>Pinia Tasks</h1>
    </header>

    <!-- New Task Form -->
    <div class="new-task-form">
      <TaskForm />
    </div>

    <!-- Filter -->
    <nav class="filter">
      <button @click="filter = 'all'">All Tasks</button>
      <button @click="filter = 'favs'">Favs Tasks</button>
    </nav>

    <!-- Loading -->
    <div class="loading" v-if="isLoading">Loading tasks...</div>

    <!-- task list -->
    <div class="task-list" v-if="filter === 'all'">
      <p>You have {{ totalCount }} tasks left to do</p>
      <div v-for="task in tasks">
        <TaskDetails :task="task" />
      </div>
    </div>
    <div class="task-list" v-if="filter === 'favs'">
      <p>You have {{ favCount }} favs tasks left to do</p>
      <div v-for="task in favs">
        <TaskDetails :task="task" />
      </div>
    </div>

    <button class="reset-store" @click="taskStore.$reset">Reset Store</button>
  </main>
</template>

<script>
  import { ref } from 'vue'
  import TaskDetails from './components/TaskDetails.vue'
  import TaskForm from './components/TaskForm.vue'
  import { useTaskStore } from './stores/TaskStore'
import { storeToRefs } from 'pinia'

  export default {
    components: { TaskDetails, TaskForm },
    setup() {
      const taskStore = useTaskStore()
      const { tasks, isLoading, favs, totalCount, favCount } = storeToRefs(taskStore)

      const filter = ref('all')

      taskStore.getTasks()

      return { taskStore, filter, tasks, isLoading, favs, totalCount, favCount }
    }
  }
</script>

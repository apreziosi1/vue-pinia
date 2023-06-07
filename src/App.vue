<template>
  <main>
    <header>
      <img src="./assets/logo.png" alt="Vue-logo">
      <h1>Vue + Pinia</h1>
    </header>

    <div class="new-task-form">
      <TaskForm />
    </div>

    <nav class="filter">
      <button @click="filter = 'all'">All tasks</button>
      <button @click="filter = 'favs'">Fav tasks</button>
      <button @click="taskStore.$reset">Reset tasks</button>
    </nav>

    <div class="loading" v-if="loading">
      Loading...
    </div>

    <div class="task-list" v-if="filter === 'all'">
      <p>You have {{ totalCount }} tasks left to do</p>
      <div v-for="task in tasks" :key="task.id">
        <TaskDetails :task="task" />
      </div>
    </div>

    <div class="task-list" v-if="filter === 'favs'">
      <p>You have {{ favCount }} fav tasks left to do</p>
      <div v-for="task in favs" :key="task.id">
        <TaskDetails :task="task" />
      </div>
    </div>

  </main>
</template>

<script>
import { ref } from 'vue'
import './assets/main.css'
import TaskDetails from './components/TaskDetails.vue'
import { useTaskStore } from './stores/TaskStore'
import TaskForm from './components/TaskForm.vue'
import { storeToRefs } from 'pinia'

export default {
  components: { TaskDetails, TaskForm },
  setup() {
    const taskStore = useTaskStore()

    const { tasks, loading, totalCount, favCount } = storeToRefs(taskStore)

    taskStore.getTasks()

    const filter = ref('all')

    return { taskStore, filter }
  }
}
</script>

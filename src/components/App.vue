<template>
  <div id="app">
    <h1>Tasker</h1>
    <TaskForm @add-task="addTask" />
    <select v-model="filter">
      <option value="all">All</option>
      <option value="completed">Completed</option>
      <option value="incomplete">Incomplete</option>
    </select>
    <ul>
      <TaskItem
        v-for="task in filteredTasks"
        :key="task.id"
        :task="task"
        @toggle-task="toggleTask"
        @remove-task="removeTask"
        @edit-task="editTask"
      />
    </ul>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'
import TaskForm from './TaskForm.vue'
import TaskItem from './TaskItem.vue'

interface Task {
  id: number
  text: string
  completed: boolean
}

export default defineComponent({
  name: 'App',
  components: {
    TaskForm,
    TaskItem,
  },
  data() {
    return {
      tasks: [] as Task[],
      filter: 'all' as 'all' | 'completed' | 'incomplete',
      taskIdCounter: 1,
    }
  },
  computed: {
    filteredTasks(): Task[] {
      if (this.filter === 'completed') return this.tasks.filter(t => t.completed)
      if (this.filter === 'incomplete') return this.tasks.filter(t => !t.completed)
      return this.tasks
    },
  },
  methods: {
    addTask(text: string) {
      this.tasks.push({
        id: this.taskIdCounter++,
        text: text,
        completed: false,
      })
    },
    removeTask(taskId: number) {
      this.tasks = this.tasks.filter(t => t.id !== taskId)
    },
    toggleTask(updatedTask: Task) {
      const index = this.tasks.findIndex(t => t.id === updatedTask.id)
      if (index !== -1) {
        this.tasks[index].completed = updatedTask.completed
      }
    },
    editTask(updatedTask: Task) {
      const index = this.tasks.findIndex(t => t.id === updatedTask.id)
      if (index !== -1) {
        this.tasks[index].text = updatedTask.text
      }
    },
  },
})
</script>

<style>
#app {
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
  font-family: Avenir, Helvetica, Arial, sans-serif;
}

ul {
  list-style-type: none;
  padding: 0;
}

select {
  margin: 10px 0;
  padding: 5px;
  font-size: 16px;
}
</style>

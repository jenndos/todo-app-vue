<template>
  <div class="input-container">
    <input
      v-model="newTask"
      placeholder="Add todo..."
      @keyup.enter="onAddTask"
    />
    <button @click="onAddTask">Add</button>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'

export default defineComponent({
  name: 'TaskForm',
  data() {
    return {
      newTask: '',
    }
  },
  methods: {
    onAddTask() {
      if (this.newTask.trim()) {
        this.$emit('add-task', this.newTask)
        this.newTask = ''
      }
    },
  },
})
</script>

<style scoped>
.input-container {
  display: flex;
  align-items: center;
  gap: 10px;
}

input {
  padding: 5px;
  font-size: 16px;
}

button {
  cursor: pointer;
  padding: 8px 10px;
  border: none;
  border-radius: 5px;
  background-color: #4caf50;
  color: white;
}

button:hover {
  background-color: #45a049;
}
</style>

<template>
  <li @click="toggleTask" :style="{ backgroundColor: task.completed ? '#e3ffd1' : '#ffd8d8' }">
    <div class="task-content">
      <input type="checkbox" :checked="task.completed" @click.stop="toggleTask" />
      <span v-if="!isEditing" :class="{ completed: task.completed }">
        {{ task.text }}
      </span>
      <input
        v-else
        type="text"
        v-model="editedText"
        @keyup.enter="saveEdit"
        @blur="saveEdit"
      />
    </div>
    <div class="action-buttons">
      <button class="editButton" @click.stop="enableEdit" v-if="!isEditing">
        Edit
      </button>
      <button class="removeIcon" @click.stop="removeTask">X</button>
    </div>
  </li>
</template>

<script lang="ts">
import { defineComponent } from 'vue'

interface Task {
  id: number
  text: string
  completed: boolean
}

export default defineComponent({
  name: 'TaskItem',
  props: {
    task: {
      type: Object as () => Task,
      required: true,
    },
  },
  data() {
    return {
      isEditing: false,
      editedText: this.task.text,
    }
  },
  methods: {
    toggleTask() {
      this.$emit('toggle-task', { ...this.task, completed: !this.task.completed })
    },
    removeTask() {
      this.$emit('remove-task', this.task.id)
    },
    enableEdit() {
      this.isEditing = true
      this.editedText = this.task.text
      this.$nextTick(() => {
        const inputEl = this.$el.querySelector('input[type="text"]')
        if (inputEl) (inputEl as HTMLElement).focus()
      })
    },
    saveEdit() {
      if (this.editedText.trim() !== this.task.text && this.editedText !== '') {
        this.$emit('edit-task', { ...this.task, text: this.editedText.trim() })
      }
      this.isEditing = false
    },
  },
})
</script>

<style scoped>
li {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin: 10px 0;
  padding: 10px;
  border-radius: 5px;
  cursor: pointer;
  background-color: #ffd8d8;
}

.task-content {
  display: flex;
  align-items: center;
  flex: 1;
}

input[type='checkbox'] {
  margin-right: 10px;
  cursor: pointer;
}

span {
  flex: 1;
  text-align: left;
}

.completed {
  text-decoration: line-through;
  color: gray;
}

.action-buttons {
  display: flex;
  gap: 10px;
}

button {
  cursor: pointer;
  padding: 5px 10px;
  border: none;
  border-radius: 5px;
  color: white;
}

.editButton {
  background-color: #2196f3;
}

.editButton:hover {
  background-color: #1976d2;
}

.editButton:active {
  background-color: #0d47a1;
}

.removeIcon {
  background-color: #ff4444;
}

.removeIcon:hover {
  background-color: #cc0000;
}

.removeIcon:active {
  background-color: #990000;
}
</style>

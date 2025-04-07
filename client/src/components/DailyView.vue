<template>
  <div>
    <h3>Today: {{ currentDateFormatted }}</h3>
    <table>
      <thead>
        <tr>
          <th>Task</th>
          <th>Done</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="task in tasks" :key="task.name">
          <td>{{ task.name }}</td>
          <td>
            <input type="checkbox" v-model="checked[task.name]" />
          </td>
        </tr>
      </tbody>
    </table>
    <button @click="submitTasks">Submit</button>
  </div>
</template>

<script lang="ts" setup>
import { ref, computed } from 'vue';

// Define the structure for the daily tasks
interface Task {
  name: string;
}

// List of tasks for the day
const tasks: Task[] = [
  { name: "Fajr Prayer" },
  { name: "Dhuhr Prayer" },
  { name: "Asr Prayer" },
  { name: "Maghrib Prayer" },
  { name: "Isha Prayer" },
  { name: "Read Qur'an" },
  { name: "Work on Project" }
];

// Use a reactive object to track checked tasks
const checked = ref<Record<string, boolean>>({});

// Get the current date
const currentDate = new Date();

// Format the current date for display
const currentDateFormatted = computed(() => {
  return currentDate.toLocaleDateString('en-US', { weekday: 'long', month: 'short', day: 'numeric' });
});

// Emit the checked tasks to the parent component
const submitTasks = () => {
  const completedTasks = tasks.filter(task => checked.value[task.name]);
  // Emit the completed tasks to the parent component
  emit('update-weekly', completedTasks);
};
</script>

<style scoped>
table {
  width: 100%;
  border-collapse: collapse;
}

th, td {
  border: 1px solid #ddd;
  padding: 0.75rem;
  text-align: center;
}

th {
  background-color: #f4f4f4;
}

input[type="checkbox"] {
  transform: scale(1.2);
}

button {
  margin-top: 1rem;
  padding: 0.5rem 1rem;
  background-color: #4CAF50;
  color: white;
  border: none;
  cursor: pointer;
}

button:hover {
  background-color: #45a049;
}
</style>

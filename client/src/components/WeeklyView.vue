<template>
  <div>
    <h3>Week: {{ weekStart }} - {{ weekEnd }}</h3>
    <table>
      <thead>
        <tr>
          <th>Task</th>
          <th v-for="day in days" :key="day">{{ day }}</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="task in allTasks" :key="task.name">
          <td>{{ task.name }}</td>
          <td v-for="(dayTasks, index) in dayColumns" :key="index">
            <!-- Check if the task is completed on that day -->
            <span v-if="dayTasks.includes(task.name)">✅</span>
            <span v-else>⬜</span>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script lang="ts" setup>
import { computed } from 'vue';

// Define types for the completed tasks
interface Task {
  name: string;
}

// Days of the week
const days = ["Mon", "Tue", "Wed", "Thu", "Fri"];

// Props to receive completed tasks for each day (from the parent)
const props = defineProps<{
  dailyTasks: Record<string, string[]>; // A map of day -> task names (completed tasks for that day)
}>();

// List of all tasks (prayer times, project tasks, etc.)
const allTasks: Task[] = [
  { name: "Fajr Prayer" },
  { name: "Dhuhr Prayer" },
  { name: "Asr Prayer" },
  { name: "Maghrib Prayer" },
  { name: "Isha Prayer" },
  { name: "Read Qur'an" },
  { name: "Work on Project" }
];

// Generate the weekly columns with completed tasks for each day
const dayColumns = computed(() => {
  return days.map(day => props.dailyTasks[day] || []);
});

// Format the start and end date for display
const currentDate = new Date();

const getStartOfWeek = (date: Date): Date => {
  const startOfWeek = new Date(date);
  const day = startOfWeek.getDay();
  const diff = day === 0 ? -6 : 1 - day;  // Sunday = 0, Monday = 1
  startOfWeek.setDate(startOfWeek.getDate() + diff);
  return startOfWeek;
};

const getEndOfWeek = (startOfWeek: Date): Date => {
  const endOfWeek = new Date(startOfWeek);
  endOfWeek.setDate(startOfWeek.getDate() + 6);
  return endOfWeek;
};

const startOfWeek = computed<Date>(() => getStartOfWeek(currentDate));
const endOfWeek = computed<Date>(() => getEndOfWeek(startOfWeek.value));

const weekStart = computed<string>(() => {
  return `${startOfWeek.value.toLocaleDateString('en-US', { month: 'short', day: 'numeric' })}`;
});

const weekEnd = computed<string>(() => {
  return `${endOfWeek.value.toLocaleDateString('en-US', { month: 'short', day: 'numeric' })}`;
});
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

span {
  font-size: 1.5rem;
}
</style>

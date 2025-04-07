<template>
  <div class="app-container">
    <Sidebar @view-change="view = $event" />
    <main class="main-content">
      <!-- Render DailyView and WeeklyView dynamically based on the selected view -->
      <DailyView v-if="view === 'daily'" @update-weekly="updateWeekly" />
      <WeeklyView v-else :dailyTasks="dailyTasks" />
    </main>
  </div>
</template>

<script lang="ts" setup>
import { ref } from 'vue';
import "./style.css";
// Import components
import Sidebar from './components/Sidebar.vue';
import DailyView from './components/DailyView.vue';
import WeeklyView from './components/WeeklyView.vue';

const view = ref<'daily' | 'weekly'>('daily');

// Store completed tasks for each day of the week
const dailyTasks = ref<Record<string, string[]>>({
  Mon: [],
  Tue: [],
  Wed: [],
  Thu: [],
  Fri: []
});

// Update the weekly tasks with the completed tasks from the daily view
const updateWeekly = (completed: string[]) => {
  const today = new Date().toLocaleString('en-US', { weekday: 'short' });
  // Update the tasks for today
  dailyTasks.value[today] = completed;
};
</script>

<style scoped>
.app-container {
  display: flex;
  min-height: 100vh;
  background: #f4f4f4;
  color: #333;
  width: 100vw;
}

.main-content {
  flex: 1;
  padding: 2rem;
}
</style>

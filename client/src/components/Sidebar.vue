<template>
    <div class="sidebar">
      <h2>📅 Schedule</h2>
      <button @click="$emit('view-change', 'daily')">Daily</button>
      <button @click="$emit('view-change', 'weekly')">Weekly</button>
    </div>
  </template>
  
  <script lang="ts" setup></script>
  
  <style scoped>
  .sidebar {
    width: 200px;
    color: white;
    background: purple;
    padding: 1.5rem;
    box-shadow: 2px 0 6px rgba(0, 0, 0, 0.1);
  }
  
  .sidebar button {
    display: block;
    margin: 1rem 0;
    background: none;
    border: none;
    cursor: pointer;
    font-size: 16px;
    padding: 0.5rem;
  }
  .sidebar button:hover {
    /* background-color: #eee; */
  }
  </style>
  
<script setup>
import ProblemStatus from './components/ProblemStatus.vue';
</script>

<template>
  <div id="app">
    <h1 style="text-align: center;">BOJ Problem Status Checker</h1>
    <ProblemStatus />
  </div>
</template>

<style scoped>
body {
  font-family: Arial, sans-serif;
  margin: 0;
  padding: 0;
  background-color: #f5f5f5;
}
h1 {
  color: #333;
  margin-top: 20px;
}
</style>

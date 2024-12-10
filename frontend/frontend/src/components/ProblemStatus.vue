<script setup>
import { ref } from 'vue';
import axios from 'axios';

const problemId = ref('');
const problemStatus = ref([]);
const loading = ref(false);
const error = ref('');

const fetchProblemStatus = async () => {
  if (!problemId.value.trim()) {
    error.value = 'Problem ID를 입력하세요.';
    return;
  }

  try {
    error.value = '';
    loading.value = true;
    problemStatus.value = [];

    const response = await axios.get(
      `http://localhost:8080/problem-status?problemId=${problemId.value.trim()}`
    );
    problemStatus.value = response.data;
  } catch (e) {
    error.value = '데이터를 가져오는 중 오류가 발생했습니다.';
  } finally {
    loading.value = false;
  }
};
</script>

<template>
  <div class="container">
    <div class="form-group">
      <label for="problemId">Problem ID:</label>
      <input
        type="text"
        id="problemId"
        v-model="problemId"
        placeholder="예: 1000"
      />
      <button @click="fetchProblemStatus" :disabled="loading">
        확인
      </button>
    </div>

    <div v-if="error" class="error">{{ error }}</div>

    <div v-if="loading" class="loading">로딩 중...</div>

    <div v-if="problemStatus.length > 0" class="result">
      <table>
        <thead>
          <tr>
            <th>사용자 이름</th>
            <th>풀이 여부</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="user in problemStatus" :key="user.username">
            <td>{{ user.username }}</td>
            <td>
              <span
                :class="{
                  solved: user.solved,
                  unsolved: !user.solved
                }"
              >
                {{ user.solved ? '풀었음' : '풀지 않음' }}
              </span>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<style scoped>
.container {
  max-width: 600px;
  margin: 0 auto;
  background: white;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  margin-top: 30px;
}
.form-group {
  display: flex;
  gap: 10px;
  margin-bottom: 20px;
}
input {
  flex: 1;
  padding: 8px;
  font-size: 16px;
}
button {
  padding: 8px 16px;
  font-size: 16px;
  background: #007bff;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}
button:disabled {
  background: #aaa;
}
.error {
  color: red;
  margin-top: 10px;
}
.loading {
  text-align: center;
  font-size: 18px;
  color: #333;
}
.result table {
  width: 100%;
  border-collapse: collapse;
}
.result th,
.result td {
  border: 1px solid #ddd;
  padding: 8px;
  text-align: left;
}
.result th {
  background: #f9f9f9;
  font-weight: bold;
}
.solved {
  color: green;
  font-weight: bold;
}
.unsolved {
  color: red;
  font-weight: bold;
}
</style>

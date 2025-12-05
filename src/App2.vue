<script setup>
import { onMounted, ref } from "vue";

const name = ref("John Doe");
const status = ref("active");
const tasks = ref(["task1", "task2", "task3"]);
const newTask = ref("");
const toggleStatus = () => {
  if (status.value === "active") {
    status.value = "pending";
  } else {
    status.value = "active";
  }
};

const addTask = () => {
  if (newTask.value.trim() !== "") {
    tasks.value.push(newTask.value);
    newTask.value = "";
  }
};
const deleteTask = (index) => {
  tasks.value.splice(index, 1);
};

onMounted(async () => {
  try {
    const response = await fetch("https://jsonplaceholder.typicode.com/todos");
    const data = await response.json();
    tasks.value = data.map((task) => task.title);
  } catch (error) {
    console.log("Error fetching tasks:", error);
  }
});
</script>

<template>
  <h1>Hello {{ name }}</h1>
  <p v-if="status === 'active'">User is Active</p>
  <p v-else-if="status === 'pending'">User is Inactive</p>
  <p v-else>User status is Unknown</p>
  <form @submit.prevent="addTask">
    <label for="newTask">AddTask</label>
    <input id="newTask" name="newTask" v-model="newTask" />
    <button type="submit">Submit</button>
  </form>
  <h3>Tasks</h3>
  <ul>
    <li v-for="(task, index) in tasks" :key="task">
      <span>{{ task }}</span>
      <button @click="deleteTask(index)">X</button>
    </li>
  </ul>

  <button @click="toggleStatus">Change status</button>
</template>

<style scoped>
h1 {
  color: #42b983;
}
</style>

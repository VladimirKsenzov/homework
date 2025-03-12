<script setup>
import { ref } from 'vue';

const tasks = ref([]);

const addTask = () => {
  tasks.value.push({
    id: tasks.value.length + 1,
    name: 'Текст задачи',
    createdAt: new Date().toLocaleDateString(),
  });
};
</script>

<template>
  <div>
    <button class="button" @click="addTask">Добавить задачу</button>
    <ul class="list">
      <li class="list__item">id</li>
      <li class="list__item">name</li>
      <li class="list__item">date</li>
    </ul>
    <ul class="tasks">
      <li v-for="task in tasks" :key="task.id" class="tasks__item">
        {{ task.id }}. {{ task.name }} (Создано: {{ task.createdAt }})
      </li>
    </ul>
  </div>
</template>

<style scoped>
li {
  list-style: none;
}

.list {
  display: flex;
  gap: 10px;
}

.tasks {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.tasksitem .listitem {
  padding: 10px;
  border-radius: 10px;
  border: 1px solid #fff;
}

.list__item:last-child {
  margin-left: 45px;
}

.button {
  transition: 0.4s ease;
}

.button:hover {
  transform: translateY(-5px);
  border-color: #afb0ca;
}
</style>

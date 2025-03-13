<script setup>
import { ref, watch, computed } from 'vue';

const taskName = ref('');
const taskDescription = ref('');

const tasks = ref([]);

const addButtonColor = ref('lightblue');
const addButtonCursor = ref('default');

watch([taskName, taskDescription], ([newName, newDescription]) => {
  if (newName && newDescription) {
    addButtonColor.value = 'blue';
    addButtonCursor.value = 'pointer';
  } else if (newName || newDescription) {
    addButtonColor.value = 'dodgerblue';
    addButtonCursor.value = 'default';
  } else {
    addButtonColor.value = 'lightblue';
    addButtonCursor.value = 'default';
  }
});

const addTask = () => {
  if (taskName.value && taskDescription.value) {
    tasks.value.push({
      id: tasks.value.length + 1,
      name: taskName.value,
      description: taskDescription.value,
      createdAt: new Date().toLocaleDateString(),
      completed: false,
    });

    taskName.value = '';
    taskDescription.value = '';
  }
};

const toggleTaskCompletion = (id) => {
  const task = tasks.value.find((task) => task.id === id);
  if (task) {
    task.completed = !task.completed;
  }
};

const removeTask = (id) => {
  tasks.value = tasks.value.filter((task) => task.id !== id);
};

// /////////////// Calc

const firstNum = ref(0);
const secondNum = ref(0);
const selectedAction = ref('Plus');

const calc = computed(() => {
  switch (selectedAction.value) {
    case 'Plus':
      return +firstNum.value + +secondNum.value;
    case 'Minus':
      return +firstNum.value - +secondNum.value;
    case 'Multiply':
      return +firstNum.value * +secondNum.value;
    case 'Divide':
      return secondNum.value !== 0
        ? +firstNum.value / +secondNum.value
        : 'Ошибка';
  }
});

const clear = () => {
  firstNum.value = 0;
  secondNum.value = 0;
  selectedAction.value = 'Plus';
};
</script>

<template>
  <div class="todo">
    <h1>Todo List</h1>
    <div class="todo__add">
      <input
        class="todo__add_input"
        v-model="taskName"
        type="text"
        placeholder="Название задачи"
      />

      <input
        class="todo__add_input"
        v-model="taskDescription"
        type="text"
        placeholder="Описание задачи"
      />

      <button
        class="todo__add_button"
        @click="addTask"
        :style="{
          backgroundColor: addButtonColor,
          cursor: addButtonCursor,
        }"
      >
        Добавить задачу
      </button>
    </div>

    <ul class="todo__list">
      <li
        v-for="task in tasks"
        class="todo__list_item"
        :key="task.id"
        :class="{ completed: task.completed }"
      >
        <div class="todo__task_info">
          <span class="todo__task_id">{{ task.id }}.</span>
          <span class="todo__task_name">{{ task.name }}</span>
          <span class="todo__task_created">({{ task.createdAt }})</span>
          <p class="todo__task_desc">{{ task.description }}</p>
        </div>
        <div class="todo__task_actions">
          <button @click="toggleTaskCompletion(task.id)">
            {{ task.completed ? 'Отменить' : 'Выполнено' }}
          </button>
          <button @click="removeTask(task.id)">Удалить</button>
        </div>
      </li>
    </ul>
  </div>
  <!-- Calc -->
  <h1>Calculator</h1>
  <div class="count">
    <input class="count__input" v-model="firstNum" type="number" />
    <select class="count__select" v-model="selectedAction">
      <option class="count__select_option" value="Plus">+</option>
      <option class="count__select_option" value="Minus">-</option>
      <option class="count__select_option" value="Multiply">X</option>
      <option class="count__select_option" value="Divide">/</option>
    </select>
    <input class="count__input" v-model="secondNum" type="number" />
    <p class="count__result">
      = <span class="count__result_output">{{ calc }}</span>
    </p>
  </div>

  <button class="button" @click="clear()">Очистить</button>
</template>

<style scoped>
.todo {
  max-width: 600px;
  margin: 0 auto;
  font-family: sans-serif;
  padding: 20px;
}

.todo__add {
  display: flex;
  gap: 10px;
  margin-bottom: 20px;
}

.todo__add_input {
  flex: 1;
  padding: 8px;
  font-size: 16px;
  border-radius: 8px;
  border: none;
}

.todo__add_button {
  padding: 8px 16px;
  font-size: 16px;
  border: none;
  color: white;
  cursor: pointer;
  transition: 0.3s ease;
}

.todo__list {
  list-style-type: none;
  padding: 0;
}

.todo__list_item {
  padding: 10px;
  border-bottom: 1px solid #ccc;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.todo__list_item.completed {
  text-decoration: line-through;
  color: gray;
}

.todo__task_info {
  display: flex;
  gap: 20px;
  align-items: center;
}

.todo__task_actions button {
  margin-left: 5px;
  padding: 5px 10px;
  border: none;
  cursor: pointer;
  font-size: 14px;
}

.todo__task_actions button:first-child {
  background-color: #b8bcb8;
  color: #0f0202;
}

.todo__task_actions button:last-child {
  background-color: #ff1100;
  color: white;
}

/* Calc */

.count {
  display: flex;
  gap: 20px;
  margin-bottom: 20px;
  align-items: center;
}

.input {
  border-radius: 10px;
}

.count__input {
  padding: 10px;
  width: 220px;
  height: 21px;
  border-radius: 10px;
  font-size: 18px;
  text-align: center;
  border: none;
}

.count__select {
  width: 50px;
  padding: 8px;
  border-radius: 10px;
  border: 1px solid #ccc;
  background-color: white;
  font-size: 16px;
  color: #333;
  cursor: pointer;
}

.count__select_option {
  background: white;
  color: black;
  font-size: 16px;
  padding: 10px;
}

.button {
  float: right;
  background-color: #ff1100;
}

.count__result {
  font-size: 18px;
}

.count__result_output {
  font-size: 24px;
  padding-left: 10px;
}
</style>

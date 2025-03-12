<script setup>
import { ref } from 'vue';

const num = ref(10);
const result = ref(num.value);

const calc = (action) => {
  switch (action) {
    case 'Plus':
      result.value += 5;
      break;
    case 'Minus':
      result.value -= 5;
      break;
    case 'Multiply':
      result.value *= 2;
      break;
    case 'Divide':
      result.value /= 2;
      break;
  }
};
</script>

<template>
  <p>Число: {{ num }}</p>

  <div class="action">
    <button class="action__btn" @click="calc('Plus')">+5</button>
    <button class="action__btn" @click="calc('Minus')">-5</button>
    <button class="action__btn" @click="calc('Multiply')">*2</button>
    <button class="action__btn" @click="calc('Divide')">/2</button>
  </div>

  <p>Результат: {{ result }}</p>
</template>

<style scoped>
.action {
  display: flex;
  gap: 10px;
}

.action__btn {
  transition: 0.4s ease;
}

.action__btn:hover {
  transform: translateY(-5px);
  border-color: #afb0ca;
}
</style>

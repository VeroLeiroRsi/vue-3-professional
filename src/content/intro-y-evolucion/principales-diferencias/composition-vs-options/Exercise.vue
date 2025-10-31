<template>
  <div>
    <h2>{{ title }}</h2>
    <p>Valor: {{ counter }}</p>
    <button @click="increment">Incrementar</button>
    <br>
    <button @click="reset">Reset</button>
    <p v-if="isEven">El número es par</p>
  </div>
</template>

<script setup>
import {ref, computed, onMounted} from 'vue';
const title =  'Contador Vue 3';
const counter = ref(0);
const setLocalVlaue = value => localStorage.setItem('counter', value);



onMounted(() => {
  const storedCount = localStorage.getItem('counter') ?? '0';
  counter.value = storedCount;
})
const increment = () => {
  counter.value++;
  setLocalVlaue(counter.value);
};

const reset = () => {
  counter.value = 0;
  setLocalVlaue(counter.value);
};

const isEven = computed(() => counter.value % 2 === 0);

</script>
<style>
</style>
<template>
  <div>
    <h2>{{ title }}</h2>
    <p>Nombre: {{ name }}</p>
    <p>Edad: {{ age }}</p>
    <p>Email: {{ email }}</p>
    <button @click="updateName">Cambiar Nombre</button><br>
    <button @click="incrementAge">Incrementar Edad</button><br>
    <button @click="updateEmail">Cambiar Email</button><br>
    <button @click="reset">Resetear</button>
    <p v-if="isAdult">Es mayor de edad</p>
  </div>
</template>

<script setup lang="ts">
import {ref, computed} from 'vue';

const title =  ref('Perfil de Usuario');
const name = ref('Juan');
const age = ref(25);
const email = ref('juan@example.com');

const updateName = () => {
      name.value = 'María';
    };

    const incrementAge = () => {
      age.value++;
    };

    const updateEmail = () => {
      email.value = 'maria@example.com';
    };

    const reset = () => {
      name.value = 'Juan';
      age.value = 25;
      email.value = 'juan@example.com';
    };

const isAdult = computed(() => age.value >= 18);

</script>

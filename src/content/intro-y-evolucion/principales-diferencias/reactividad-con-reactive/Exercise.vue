<template>
  <div>
    <h2>{{ title }}</h2>
    <p>Nombre: {{ user.name }}</p>
    <p>Edad: {{ user.age }}</p>
    <p>Email: {{ user.email }}</p>
    <p>Ciudad: {{ user.address.city }}</p>
    <p>País: {{ user.address.country }}</p>
    <br>
    <button @click="updateName">Cambiar Nombre</button><br>
    <button @click="incrementAge">Incrementar Edad</button><br>
    <button @click="updateEmail">Cambiar Email</button><br>
    <button @click="updateAddress">Cambiar Dirección</button><br>
    <button @click="reset">Resetear</button>
    <br>
    <p v-if="isAdult">Es mayor de edad</p>
    <p>Total de campos: {{ totalFields }}</p>
  </div>
</template>

<script setup lang="ts" >
import {reactive, computed} from 'vue';

interface Address {
  city: string;
  country: string;
}

interface User {
  name: string;
  age: number;
  email: string;
  address: Address;
}

const title =  'Perfil de Usuario Completo';
const user = reactive<User>({
  name: 'Juan',
  age: 25,
  email: 'juan@example.com',
  address: {
    city: 'Madrid',
    country: 'España',
  },
});

const updateName = () => {
      user.name = 'María';
    };

    const incrementAge = () => {
      user.age++;
    };

    const updateEmail = () => {
      user.email = 'maria@example.com';
    };


  const updateAddress = () => {
      user.address.city = 'Barcelona';
      user.address.country = 'España';
    };
    const reset = () => {
      user.name = 'Juan';
      user.age = 25;
      user.email = 'juan@example.com';
      user.address.city = 'Madrid';
      user.address.country = 'España';
    };

const isAdult = computed(() => user.age >= 18);

const totalFields = computed(() => {
  Object.keys(user).length + Object.keys(user.address).length}); 

</script>

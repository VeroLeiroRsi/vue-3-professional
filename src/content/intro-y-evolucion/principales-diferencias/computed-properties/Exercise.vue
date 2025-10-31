<template>
  <div class="p-6 max-w-md mx-auto bg-white rounded-xl shadow-lg">
    <h2 class="text-2xl font-bold mb-6">{{ title }}</h2>

    <!-- Cart Items -->
    <div class="mb-6">
      <h3 class="text-lg font-semibold mb-3">Productos en el carrito:</h3>
      <div v-for="item in items" :key="item.id" class="flex justify-between items-center p-3 bg-gray-50 rounded mb-2">
        <div>
          <span class="font-medium">{{ item.name }}</span>
          <span class="text-gray-500"> - €{{ item.price }}</span>
        </div>
        <div class="flex items-center space-x-2">
          <button @click="decrementQuantity(item.id)" class="px-2 py-1 bg-red-500 text-white rounded text-sm">-</button>
          <span class="mx-2">{{ item.quantity }}</span>
          <button @click="incrementQuantity(item.id)" class="px-2 py-1 bg-green-500 text-white rounded text-sm">+</button>
          <button @click="removeItem(item.id)" class="px-2 py-1 bg-gray-500 text-white rounded text-sm ml-2">Eliminar</button>
        </div>
      </div>
    </div>

    <!-- Add New Item -->
    <div class="mb-6 p-4 bg-blue-50 rounded">
      <h3 class="font-semibold mb-2">Agregar producto:</h3>
      <div class="flex space-x-2">
        <input
          v-model="newItemName"
          placeholder="Nombre"
          class="flex-1 px-2 py-1 border rounded"
        />
        <input
          v-model="newItemPrice"
          type="number"
          placeholder="Precio"
          class="w-20 px-2 py-1 border rounded"
        />
        <button @click="addItem" class="px-3 py-1 bg-blue-500 text-white rounded">+</button>
      </div>
    </div>

    <!-- Add coupon-->
    <div class="mb-6 p-4 bg-blue-50 rounded">
      <h3 class="font-semibold mb-2">Añadir cupón descuento:</h3>
      <div class="flex space-x-2">

        <input
          v-model="newCouponCode"
          type="number"
          placeholder="porcentaje"
          class="w-20 px-2 py-1 border rounded"
        />
        <button @click="addCouponItem" class="px-3 py-1 bg-blue-500 text-white rounded">+</button>
      </div>
    </div>


    <!-- Cart Statistics (Manually Synchronized) -->
    <div class="bg-gray-100 p-4 rounded">
      <h3 class="font-semibold mb-2">Resumen del carrito:</h3>
      <p><strong>Total de productos:</strong> {{ totalItems }}</p>
      <p><strong>Total de artículos únicos:</strong> {{ uniqueItemsCount }}</p>
      <p><strong>Precio total:</strong> €{{ totalPrice.toFixed(2) }}</p>
      <p><strong>Precio promedio:</strong> €{{ averagePrice.toFixed(2) }}</p>
      <p v-if="hasExpensiveItems" class="text-orange-600"><strong>⚠️ Tienes productos caros (+€50)</strong></p>
      <p><strong>Estado del carrito:</strong> {{ cartStatus }}</p>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue';

// Reactive data
const title = ref('Carrito de Compras - Sincronización Manual');
const items = ref([
  { id: 1, name: 'Laptop', price: 999, quantity: 1 },
  { id: 2, name: 'Mouse', price: 25, quantity: 2 },
  { id: 3, name: 'Teclado', price: 75, quantity: 1 },
]);

const newItemName = ref('');
const newItemPrice = ref(0);
const newCouponCode = ref(0);


const totalItems = computed(() => items.value.reduce((sum, item) => sum + item.quantity, 0));

const uniqueItemsCount = computed(() => items.value.length);

const totalPrice = computed(() => items.value.reduce((sum, item) => sum + item.price * item.quantity, 0));

const averagePrice = computed(() => uniqueItemsCount.value > 0 ? totalPrice.value / uniqueItemsCount.value : 0);

const hasExpensiveItems = computed(() => items.value.some((item) => item.price > 50));
const cartStatus = computed(() => {
  if (items.value.length === 0) {
    return 'Carrito vacío';
  } else if (totalPrice.value > 500) {
    return 'Carrito premium';
  } else {
    return 'Carrito con productos';
  }
});


const incrementQuantity = (id: number) => {
  const item = items.value.find((item) => item.id === id);
  if (item) {
    item.quantity++;
  }
};

const decrementQuantity = (id: number) => {
  const item = items.value.find((item) => item.id === id);
  if (item && item.quantity > 1) {
    item.quantity--;
  }
};

const removeItem = (id: number) => {
  items.value = items.value.filter((item) => item.id !== id);
};

const addItem = () => {
  if (newItemName.value && newItemPrice.value > 0) {
    const newId = Math.max(...items.value.map((item) => item.id)) + 1;
    items.value.push({
      id: newId,
      name: newItemName.value,
      price: newItemPrice.value,
      quantity: 1,
    });
    newItemName.value = '';
    newItemPrice.value = 0;
  }
};

const addCouponItem = () => {
  if (newCouponCode.value > 0) {
    const discountPercentage = newCouponCode.value;
    items.value = items.value.map((item) => ({
      ...item,
      price: parseFloat((item.price * (1 - discountPercentage / 100)).toFixed(2)),
    }));
    const newId = Math.max(...items.value.map((item) => item.id)) + 1;
    items.value.push({
      id: newId,
      name: 'Cupón de descuento',
      price: newCouponCode.value,
      quantity: 1,
    });
    newCouponCode.value = 0;
    
  }
};
</script>

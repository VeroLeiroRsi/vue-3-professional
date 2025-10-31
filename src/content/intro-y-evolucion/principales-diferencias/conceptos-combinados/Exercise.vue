<template>
  <div class="message-formatter" :class="containerClass">
    <h2>{{ props.title }}</h2>

    <!-- Input with template ref -->
    <div class="input-section">
      <label>Escribe tu mensaje:</label>
      <input
        ref="messageInput"
        v-model="message"
        type="text"
        placeholder="Escribe algo..."
        class="input-field"
      />
      <button @click="focusInput">Enfocar Input</button>
    </div>

    <!-- Display formatted message using filters -->
    <div class="output-section">
      <h3>Mensaje formateado:</h3>
      <p class="formatted-message" :class="messageClass">
        {{ formattedMessage }}
      </p>
      <p class="char-count" :style="{ color: textColor }">
        Caracteres: {{ message.length }}
      </p>
    </div>

    <!-- Theme controls -->
    <div class="controls">
      <label>
        <input
          type="checkbox"
          v-model="isDarkMode"
        />
        Modo oscuro
      </label>

      <label>
        Tamaño del texto:
        <select v-model="fontSize">
          <option value="small">Pequeño</option>
          <option value="medium">Mediano</option>
          <option value="large">Grande</option>
        </select>
      </label>
    </div>

    <!-- Action button -->
    <div class="actions">
      <button
        @click="sendMessage"
        :disabled="!message.trim()"
        class="send-button"
      >
        Enviar Mensaje
      </button>
    </div>

    <!-- Status display -->
    <div class="status" v-if="lastSentMessage">
      <p>Último mensaje enviado: <strong>{{ lastSentMessage }}</strong></p>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, computed, watch } from 'vue';

const props = defineProps<{
  title?: string;
}>();

const emit = defineEmits<{
  (e: 'message-sent', message: string): void;
}>();

const message = ref('');
const isDarkMode = ref(false);
const fontSize = ref('medium');
const lastSentMessage = ref('');
const messageInput = ref<HTMLInputElement | null>(null);



const containerClass = computed(() => (isDarkMode.value ? 'dark-mode' : 'light-mode'));
const textColor = computed(() => (isDarkMode.value ? '#ecf0f1' : '#333333'));
const messageClass = computed(() => {
  return {
    'has-content': message.value.trim().length > 0,
    'long-message': message.value.length > 50,
  };
});
const formattedMessage = computed(() => {
  let msg = message.value.trim();
  if (msg.length === 0) return 'No hay mensaje';

  // Capitalize first letter
  msg = msg.charAt(0).toUpperCase() + msg.slice(1);

  // Add exclamation if long message
  if (msg.length > 50) {
    msg += ' !!!';
  }

  return msg;
});

const fontSizeValue = computed(() => {
  switch (fontSize.value) {
    case 'small':
      return '14px';
    case 'large':
      return '20px';
    case 'medium':
    default:
      return '16px';
  }
}); 

const focusInput = () => {
  messageInput.value?.focus();
};
const sendMessage = () => {
  if (message.value.trim()) {
    lastSentMessage.value = message.value.trim();
    emit('message-sent', lastSentMessage.value);
    message.value = '';
  }
};      


</script>

<style scoped>
.message-formatter {
  max-width: 500px;
  margin: 0 auto;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  font-size: v-bind(fontSizeValue);
  transition: all 0.3s ease;
}

.light-mode {
  background-color: #ffffff;
  color: #333333;
}

.dark-mode {
  background-color: #2c3e50;
  color: #ecf0f1;
}

.input-section {
  margin-bottom: 20px;
}

.input-field {
  width: 100%;
  padding: 8px 12px;
  margin: 8px 0;
  border: 1px solid #ddd;
  border-radius: 4px;
  font-size: inherit;
}

.dark-mode .input-field {
  background-color: #34495e;
  border-color: #7f8c8d;
  color: #ecf0f1;
}

.output-section {
  margin-bottom: 20px;
}

.formatted-message {
  padding: 10px;
  background-color: #f8f9fa;
  border-radius: 4px;
  font-weight: bold;
  min-height: 20px;
}

.dark-mode .formatted-message {
  background-color: #34495e;
}

.formatted-message.has-content {
  border-left: 4px solid #3498db;
}

.formatted-message.long-message {
  border-left-color: #e74c3c;
}

.char-count {
  font-size: 0.9em;
  margin-top: 5px;
  color: v-bind(textColor);
}

.controls {
  margin-bottom: 20px;
}

.controls label {
  display: block;
  margin-bottom: 10px;
}

.controls select {
  margin-left: 8px;
  padding: 4px 8px;
}

.actions {
  margin-bottom: 20px;
}

.send-button {
  background-color: #3498db;
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 4px;
  cursor: pointer;
  font-size: inherit;
}

.send-button:hover:not(:disabled) {
  background-color: #2980b9;
}

.send-button:disabled {
  background-color: #bdc3c7;
  cursor: not-allowed;
}

.status {
  background-color: #d4edda;
  border: 1px solid #c3e6cb;
  color: #155724;
  padding: 10px;
  border-radius: 4px;
}

.dark-mode .status {
  background-color: #1e3a2e;
  border-color: #2d5a3d;
  color: #a3d9b1;
}
</style>
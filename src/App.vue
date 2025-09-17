<script setup lang="ts">
import { onMounted, ref } from 'vue';

// 1. Crear una referencia para el botón
const notifyBtn = ref<HTMLButtonElement | null>(null);

// Función para solicitar permiso y mostrar la notificación
const showNotification = () => {
  // Comprobar si el navegador soporta notificaciones
  if (!("Notification" in window)) {
    alert("Este navegador no soporta notificaciones de escritorio.");
    return;
  }

  // Si el permiso ya fue otorgado, mostrar notificación
  if (Notification.permission === "granted") {
    new Notification("¡Hola de nuevo! 👋");
  }
  // Si el permiso no ha sido denegado, solicitarlo
  else if (Notification.permission !== "denied") {
    Notification.requestPermission().then(permission => {
      // Si el usuario otorga el permiso, mostrar notificación
      if (permission === "granted") {
        new Notification("¡Gracias por permitir las notificaciones! 🎉");
      }
    });
  }
};

onMounted(() => {
  // 2. Asociar el evento click al elemento referenciado
  if (notifyBtn.value) {
    notifyBtn.value.addEventListener('click', showNotification);
  }
});
</script>

<template>
  <h1>Ejemplo de Notificaciones Push</h1>
  <button ref="notifyBtn">Mostrar Notificación</button>
</template>

<style scoped></style>

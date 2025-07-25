<script setup>
import { ref, defineEmits } from 'vue';

const emits = defineEmits(["onNewItem"]);
const text = ref("");
const hasError = ref(false)

// Función corregida
function handleSubmit() {
  if (text.value.trim() !== "") {
    emits("onNewItem", text.value); // Emitir el valor del texto
    text.value = ""; // Limpiar el input
  }
}
function VerificarError(){
    if (text.value.trim() === ""){
        hasError.value = true;
    } else {
        hasError.value = false;
    }

}
</script>

<template>
  <form @submit.prevent="handleSubmit" @input="VerificarError">
    <input type="text" v-model="text"  placeholder="sapo" />
    <span v-if="hasError">⚠️ Escribe algo primero</span>
  </form>
</template>

<style scoped>
input {
  width: 100%;
  padding: 8px;
  border-radius: 4px;
  border: 1px solid #ccc;
}
</style>

<script setup>
function switchTheme() {
  const element = document.querySelector('html');
  element.classList.toggle('dark');
}
import 'primeicons/primeicons.css'
</script>
<template>
  <NuxtPage />
</template>

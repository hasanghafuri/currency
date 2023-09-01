<script setup>
import CardCurrency from './components/CardCurrency.vue'
</script>

<template>
  <div class="warper">
    <CardCurrency />
  </div>
</template>

<style scoped>
.warper {
  @apply flex justify-center flex-col gap-9 items-center mt-24;
}
</style>

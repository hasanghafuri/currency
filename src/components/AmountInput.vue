<script setup>
import { ref, watch } from 'vue'

const $props = defineProps({
  modelValue: Number
})
const amount = ref($props.modelValue)
const $emit = defineEmits(['update:modelValue'])

function onInput(event) {
  $emit('update:modelValue', event.target.value)
}
watch(
  () => $props.modelValue,
  () => {
    amount.value = $props.modelValue
  }
)
</script>

<template>
  <div>
    <div class="warper">
      <div class="flex items-center">
        <img class="img" src="@/assets/irt.png" alt="irt" />

        <span class="title"> تومان </span>
      </div>

      <input @input="onInput" class="amount" :value="amount" dir="ltr" />

      <span class="label">پرداخت </span>
    </div>
  </div>
</template>

<style scoped>
.warper {
  @apply flex items-center justify-between bg-[#fafafa] px-3 py-4 rounded-lg;
}
.amount {
  @apply px-3 py-4 border-none w-full bg-[#fafafa];
}
.amount:focus {
  outline: none;
}
.label {
  @apply text-gray-500 text-opacity-80 font-bold;
}
.title {
  @apply px-4  opacity-90;
}
.img {
  @apply w-5 h-5 rounded-full;
}
</style>

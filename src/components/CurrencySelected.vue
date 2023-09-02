<script setup>
import { ref, watchEffect } from 'vue'

const $emit = defineEmits(['update:modelValue'])
const $props = defineProps({ currencyOption: { type: Array, require: true }, modelValue: Object })

const selectedCurrency = ref($props.modelValue)

watchEffect(() => {
  $emit('update:modelValue', selectedCurrency.value)
})
</script>

<template>
  <div class="warper">
    <select v-model="selectedCurrency.currency" class="selectCurrency">
      <option
        v-for="option in $props.currencyOption"
        :key="option.value"
        :value="option.value"
        dir="rtl"
        class="text-center"
      >
        {{ option.label }}
      </option>
    </select>

    <input class="amountCurrency" v-model="selectedCurrency.amount" dir="ltr" />

    <span class="label">دریافت</span>
  </div>
</template>

<style scoped>
.warper {
  @apply flex items-center justify-center px-3 py-2 rounded-lg bg-[#fafafa];
}
.label {
  @apply text-gray-500 text-opacity-80 font-bold;
}

.selectCurrency {
  @apply border-none bg-[#fafafa]  rounded-lg p-2;
}
.amountCurrency {
  @apply border-none bg-[#fafafa] w-full px-3 py-2;
}

.selectCurrency:focus {
  outline: none;
}
</style>

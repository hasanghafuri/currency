<script setup>
import { computed, ref } from 'vue'
import AmountInput from './AmountInput.vue'
import CurrencySelected from './CurrencySelected.vue'

const currencyOption = ref([
  { value: 'lit', label: 'لیتکوین', pricePerUnit: 50023 },
  { value: 'eth', label: 'اتریوم', pricePerUnit: 85857559 },
  { value: 'bitcoin', label: 'بیت کوین', pricePerUnit: 1368665805 }
])

const amount = ref(0)
const selectedCurrency = ref({ currency: 'bitcoin', amount: 0 })

const currency = computed(() => {
  return currencyOption.value.find((option) => option.value === selectedCurrency.value.currency)
})

function createDebounce() {
  let timeout = null
  return function (fnc, delayMs) {
    clearTimeout(timeout)
    timeout = setTimeout(() => {
      fnc()
    }, delayMs || 500)
  }
}

const disableCard = ref(true)
const showBill = ref(false)
function showCartBill() {
  if (!amount.value || !selectedCurrency.value) {
    showBill.value = false
  } else {
    showBill.value = true
    disableCard.value = false
  }
}

function onAmountInput() {
  const debouncedFn = createDebounce()
  debouncedFn(() => {
    selectedCurrency.value.amount = +amount.value / currency.value.pricePerUnit
  })
}
function onSelectedCurrency() {
  const debouncedFn = createDebounce()
  debouncedFn(() => {
    amount.value = +selectedCurrency.value.amount * currency.value.pricePerUnit
  })
}

const showSuccessMessage = ref(false)
function showMessage() {
  showSuccessMessage.value = true
  setTimeout(() => {
    showBill.value = false
    disableCard.value = true
    amount.value = 0
    selectedCurrency.value.amount = 0
  }, 2000)
}
</script>

<template>
  <div v-if="disableCard" class="card">
    <AmountInput @input="onAmountInput" v-model="amount" />
    <CurrencySelected
      @input="onSelectedCurrency"
      v-model="selectedCurrency"
      :currencyOption="currencyOption"
    />

    <div class="PriceEveryDay">
      <h1>
        {{ currency.pricePerUnit.toLocaleString() }}

        <span class="price">:قیمت هر روز / {{ currency.label }}</span>
      </h1>
    </div>

    <button @click="showCartBill" class="buy">خرید</button>
  </div>

  <div class="bill">
    <div class="w-full p-8">
      <div class="BillPayment">
        <h1 class="label">قبض خرید و پرداخت</h1>
      </div>

      <div class="flex justify-between items-center mt-8">
        <span class=" ">{{ amount.toLocaleString() }}</span>

        <span class="label">پرداخت</span>
      </div>

      <div class="flex justify-between items-center mt-8">
        <span>{{ selectedCurrency.amount }}</span>

        <span class="label">دریافت</span>
      </div>

      <div class="flex justify-end mt-8">
        <input @click="showMessage" class="checkbox" type="checkbox" />
      </div>

      <div v-if="showSuccessMessage" class="boxSuccess">
        <h1 class="text-center font-bold text-white">عملیات موفق امیز بود</h1>
      </div>
    </div>
  </div>
</template>
<style scoped>
.card {
  @apply bg-white shadow-xl rounded-lg p-10 flex flex-col gap-12 w-1/2;
}

.buy {
  @apply p-4 text-center rounded-lg text-white font-bold bg-[#21bf73];
}

.bill {
  @apply bg-white shadow-lg rounded-lg flex flex-col w-80;
}
.label {
  @apply text-gray-500 text-opacity-80 font-bold;
}
.PriceEveryDay {
  @apply flex justify-center items-center;
}
.price {
  @apply mr-4 font-bold opacity-80 text-gray-500;
}
.close {
  @apply absolute left-0 top-0;
}
.BillPayment {
  @apply flex justify-center items-center;
}
.checkbox {
  @apply text-right cursor-pointer;
}
.boxSuccess {
  @apply mt-5 px-2 opacity-80 shadow-md py-3 bg-[#21bf73] rounded-lg;
}
</style>

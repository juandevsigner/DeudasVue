<script setup>
import { ref, computed, watch } from "vue";
import Header from "./components/Header.vue";
import Button from "./components/Button.vue";
import { calcularTotalPago } from "./helpers";

const cantidad = ref(0);
const meses = ref(6);
const total = ref(calcularTotalPago(cantidad.value, meses.value));
const MIN = 0;
const MAX = 20000;
const STEP = 100;

const formatearDinero = valor => {
  const formatter = new Intl.NumberFormat("en-US", {
    style: "currency",
    currency: "USD",
  });

  return formatter.format(valor);
};

const pagoMensual = computed(() => {
  return total.value / meses.value;
});

watch([cantidad, meses], () => {
  total.value = calcularTotalPago(cantidad.value, meses.value);
});

const handleChangeDecremento = () => {
  const valor = cantidad.value - STEP;
  if (valor < MIN) {
    return;
  }
  cantidad.value = valor;
};
const handleChangeIncremento = () => {
  const valor = cantidad.value + STEP;
  if (valor > MAX) {
    return;
  }
  cantidad.value = valor;
};
</script>

<template>
  <div class="my-20 max-w-lg mx-auto bg-white shadow-lg p-10 rounded-lg">
    <Header />
    <div class="flex justify-between mt-10">
      <Button :operador="'-'" @fn="handleChangeDecremento" />
      <Button :operador="'+'" @fn="handleChangeIncremento" />
    </div>
    <div class="my-5">
      <input
        class="w-full bg-gray-200 accent-indigo-500 hover:accent-indigo-700"
        type="range"
        :min="MIN"
        :max="MAX"
        :step="STEP"
        v-model.number="cantidad"
      />
      <p class="text-center my-10 text-5xl font-bold text-indigo-500">
        {{ formatearDinero(cantidad) }}
      </p>
      <h2 class="text-2xl font-bold text-gray-500 text-center">
        Elige un <span class="text-indigo-600">Plazo a pagar</span>
      </h2>
      <select
        class="w-full pd-2 mt-5 bg-white border border-gray-300 rounded-lg text-center text-xl font-bold text-gray-500"
        name=""
        id=""
        :value="meses"
        v-model.number="meses"
      >
        <option value="6">6 Meses</option>
        <option value="12">12 Meses</option>
        <option value="24">24 Meses</option>
      </select>
    </div>
    <div v-if="total > 0" class="my-5 space-y-3 bg-gray-50 rounded-lg p-5">
      <h2 class="text-2xl font-bold text-gray-500 text-center">
        Resumen <span class="text-indigo-600">de Pagos</span>
      </h2>
      <p class="text-xl text-gray-500 font-bold text-center">
        {{ meses }} Meses
      </p>
      <p class="text-xl text-gray-500 font-bold text-center">
        Total a pagar: {{ formatearDinero(total) }}
      </p>
      <p class="text-xl text-gray-500 font-bold text-center">
        Mensuales: {{ formatearDinero(pagoMensual) }}
      </p>
    </div>

    <p v-else class="text-center">Agregar una cantidad y un plazo a pagar</p>
  </div>
</template>

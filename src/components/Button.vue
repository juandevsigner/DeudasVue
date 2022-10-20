<script setup>
const props = defineProps({
  operador: String,
});
</script>

<template>
  <button
    class="transition-all delay-100 h-10 w-10 flex items-center justify-center font-bold rounded-full bg-lime-500 hover:outline-none hover:ring-2 hover:ring-offset-2 hover:ring-lime-500 text-white text-2xl"
    @click="$emit('fn')"
  >
    {{ operador }}
  </button>
</template>

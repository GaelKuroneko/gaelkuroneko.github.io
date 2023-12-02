<template>
  <div class="bg-gray-100 min-h-screen flex flex-col items-center justify-center">
    <!-- API de Fechas -->
    <div class="bg-white p-8 rounded shadow-md mb-8">
      <h1 class="text-3xl font-bold mb-4">Sit Motul - Fechas</h1>
      <div v-if="apii && apii.periodo" class="text-lg font-semibold mb-4">
        Periodo: {{ apii.periodo }}
      </div>
      <div class="grid grid-cols-2 gap-4">
        <div v-if="apii && apii.alumTotal" class="text-lg font-semibold">
          Total de alumnos: {{ apii.alumTotal }}
        </div>
        <div v-if="apii && apii.alumEva" class="text-lg font-semibold">
          Alumnos evaluados: {{ apii.alumEva }}
        </div>
      </div>
      <div class="flex justify-between mt-4">
        <div v-if="apii && apii.inicio" class="text-lg font-semibold">
          Inicio: {{ fechaInicio }}
        </div>
        <div v-if="apii && apii.fin" class="text-lg font-semibold">
          Fin: {{ fechaFin }}
        </div>
      </div>
      <div class="flex items-center justify-center mt-8">
        <div v-if="apii && apii.fin" class="bg-amber-300 p-4 rounded text-lg font-semibold">
          Tiempo por concluir: {{ difHoras }} horas
        </div>
      </div>
    </div>

    <!-- API de Ingenierías -->
    <div class="bg-white p-8 rounded shadow-md">
      <h1 class="text-3xl font-bold mb-4">Sit Motul - Ingenierías</h1>
      <div class="grid grid-cols-5 gap-4">
        <div class="bg-indigo-500 h-60 w-32 p-4 rounded-md text-center">
          <div v-if="aping">
            <p>ISC</p>
            <p>Faltantes: {{aping.ISC.faltantes}}</p>
            <p>Listas: {{aping.ISC.listas}}</p>
            <p class="text-4xl font-bold">{{ aping.ISC.listas !== 0 ? (100 / (aping.ISC.listas + aping.ISC.faltantes)* aping.ISC.listas).toFixed(1) + "%" : 'N/A' }} </p>
          </div>
        </div>

        <div class="bg-indigo-500 h-60 w-32 p-4 rounded-md text-center">
          <div v-if="aping">
            <p>IEM</p>
            <p>Faltantes: {{aping.IEM.faltantes}}</p>
            <p>Listas: {{aping.IEM.listas}}</p>
            <p class="text-4xl font-bold">{{ aping.IEM.listas !== 0 ? (100 / (aping.IEM.listas + aping.IEM.faltantes)* aping.IEM.listas).toFixed(1) + "%" : 'N/A' }} </p>
          </div>
        </div>

        <div class="bg-indigo-500 h-60 w-32 p-4 rounded-md text-center">
          <div v-if="aping">
            <p>IE</p>
            <p>Faltantes: {{aping.IE.faltantes}}</p>
            <p>Listas: {{aping.IE.listas}}</p>
            <p class="text-4xl font-bold">{{ aping.IE.listas !== 0 ? (100 / (aping.IE.listas + aping.IE.faltantes)* aping.IE.listas).toFixed(1) + "%" : 'N/A' }} </p>
          </div>
        </div>

        <div class="bg-indigo-500 h-60 w-32 p-4 rounded-md text-center">
          <div v-if="aping">
            <p>IER</p>
            <p>Faltantes: {{aping.IER.faltantes}}</p>
            <p>Listas: {{aping.IER.listas}}</p>
            <p class="text-4xl font-bold">{{ aping.IER.listas !== 0 ? (100 / (aping.IER.listas + aping.IER.faltantes)* aping.IER.listas).toFixed(1) + "%" : 'N/A' }} </p>
          </div>
        </div>

        <div class="bg-indigo-500 h-60 w-32 p-4 rounded-md text-center">
          <div v-if="aping">
            <p>II</p>
            <p>Faltantes: {{aping.II.faltantes}}</p>
            <p>Listas: {{aping.II.listas}}</p>
            <p class="text-4xl font-bold">{{ aping.II.listas !== 0 ? (100 / (aping.II.listas + aping.II.faltantes)* aping.II.listas).toFixed(1) + "%" : 'N/A' }} </p>
          </div>
        </div>

      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, computed } from 'vue';
import dayjs from 'dayjs';

// API de Fechas
const apii = ref({
  periodo: "",
  alumTotal: "",
  alumEva: "",
  inicio: "",
  fin: "",
});

const fechaInicio = computed(() => {
  return dayjs(apii.value.inicio).format('DD-MM-YYYY');
});

const fechaFin = computed(() => {
  return dayjs(apii.value.fin).format('DD-MM-YYYY');
});

const difHoras = computed(() => {
  const now = dayjs();
  const finDia = dayjs(apii.value.fin);
  const difHora = finDia.diff(now, 'hour');
  return difHora;
});

const fetchData = async () => {
  try {
    const response = await fetch('https://sitmotul.com.mx/api/statusEval');
    apii.value = await response.json();
  } catch (error) {
    console.log('Error al obtener datos: ', error);
  }
};

onMounted(() => {
  fetchData();
});

// API de Ingenierías
const aping = ref(null);


const fetchDataIngenierias = async () => {
  try {
    const response = await fetch('https://sitmotul.com.mx/api/statusEvalIng');
    aping.value = await response.json();
  } catch (error) {
    console.log('Error al obtener datos: ', error);
  }
};

onMounted(() => {
  fetchDataIngenierias();
});
</script>

<style scoped>
/* Agrega estilos adicionales si es necesario */
</style>

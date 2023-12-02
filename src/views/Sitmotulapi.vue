<template>
    <div class="min-h-screen flex items-center justify-center bg-gray-100">
      <div class="bg-white p-8 rounded shadow-md">
        <h1 class="text-4xl font-bold mb-4">Sit Motul</h1>
        <div v-if="apii && apii.periodo" class="text-lg font-bold mb-4">
          Periodo: {{ apii.periodo }}
        </div>
        <div class="grid grid-cols-2 gap-4">
          <div v-if="apii && apii.alumTotal" class="text-lg">
            Total de alumnos: {{ apii.alumTotal }}
          </div>
          <div v-if="apii && apii.alumEva" class="text-lg">
            Alumnos evaluados: {{ apii.alumEva }}
          </div>
        </div>
        <div class="flex justify-between mt-4">
          <div v-if="apii && apii.inicio" class="text-lg">
            Inicio: {{ fechaInicio }}
          </div>
          <div v-if="apii && apii.fin" class="text-lg">
            Fin: {{ fechaFin }}
          </div>
        </div>
        <div class="flex items-center justify-center mt-8">
          <div v-if="apii && apii.fin" class="bg-amber-300 p-4 rounded">
            Tiempo por concluir {{ difHoras }} horas
          </div>
        </div>
      </div>
    </div>
  
  
  
  </template>
  
  <script>
  import { ref, onMounted, computed } from 'vue';
  import dayjs from 'dayjs';
  
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
      const data = await response.json();
      apii.value = data;
    } catch (error) {
      console.log('Error al obtener datos: ', error);
    }
  };
  
  onMounted(() => {
    fetchData();
  });
  </script>
  
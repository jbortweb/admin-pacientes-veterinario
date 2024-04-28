<script setup>
import { onMounted, reactive, ref, watch } from "vue";
import { uid } from "uid";
import Formulario from "./components/Formulario.vue";
import Header from "./components/Header.vue";
import Paciente from "./components/Paciente.vue";

const pacientes = ref([]);

const state = reactive({
  id: null,
  nombre: "",
  propietario: "",
  email: "",
  date: "",
  sintomas: "",
});

const guardarPaciente = () => {
  if(state.id) {
    const {id} = state
    const i = pacientes.value.findIndex((pacienteState) => pacienteState.id === id)
    pacientes.value[i]={...state}
  }else {
    pacientes.value.push({...state, id:uid()});
  }

  Object.assign(state, {
    id: null,
    nombre: '',
    propietario: '',
    email: '',
    date: '',
    sintomas: ''
  });
};

const actualizarPaciente = (id) => {
   const pacienteEditar = pacientes.value.filter( paciente => paciente.id === id)[0]
   Object.assign(
    state,
    pacienteEditar
   )
};

watch(pacientes, () => {
    guardarLocalStorage()
  }, {
    deep: true
  })

  const guardarLocalStorage = () => {
    localStorage.setItem('pacientes', JSON.stringify(pacientes.value))
  }

  onMounted(() => {
    const pacientesStorage = localStorage.getItem('pacientes')
    if(pacientesStorage) {
      pacientes.value = JSON.parse(pacientesStorage)
    }
  })

  const eliminarPacientes = (id) => {
     pacientes.value = pacientes.value.filter(paciente => paciente.id !== id)
  };
  
</script>

<template>
  <div class="container mx-auto mt-10">
    <Header />

    <div class="mt-12 md:flex">
      <Formulario
        v-model:nombre="state.nombre"
        v-model:propietario="state.propietario"
        v-model:email="state.email"
        v-model:date="state.date"
        v-model:sintomas="state.sintomas"
        @guardar-paciente="guardarPaciente"
        :id="state.id"
      />
      <div class="md:w-1/2 md:h-screen overflow-y-scroll">
        <h3 class="font-black text-3xl text-center">
          Administra tus pacientes
        </h3>
        <p class="text-lg mt-5 text-center mb-10">
          Información de
          <span class="text-indigo-600 font-bold">Pacientes</span>
        </p>
        <div v-if="pacientes.length > 0">
          <Paciente
            v-for="paciente in pacientes"
            :key="paciente.id"
            :paciente="paciente"
            @actualizar-paciente="actualizarPaciente"
            @eliminar-pacientes='eliminarPacientes'
          />
        </div>
        <p v-else class="mt-10 text-2xl text-center">No hay pacientes</p>
      </div>
    </div>
  </div>
</template>
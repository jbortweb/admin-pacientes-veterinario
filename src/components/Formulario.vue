<template>
  <div class="md:w-1/2">
    <h2 class="font-black text-3xl text-center">Seguimiento de pacientes</h2>
    <p class="text-lg mt-5 text-center mb-10">
      Añade pacientes y
      <span class="text-indigo-600 font-bold">Adminístralos</span>
    </p>
    <form
      action=""
      class="bg-white shadow-md rounded-lg py-10 px-5 mb-10"
      @submit.prevent="validar"
    >
      <div class="mb-5">
        <label for="mascota" class="block text-gray-700 uppercase font-bold">
          Nombre mascota
        </label>
        <input
          type="text"
          id="mascota"
          placeholder="Nombre de la Mascota"
          class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
          :value="nombre"
          @input="$emit('update:nombre', $event.target.value)"
        />
      </div>

      <div class="mb-5">
        <label
          for="propietario"
          class="block text-gray-700 uppercase font-bold"
        >
          Nombre Propietario
        </label>
        <input
          type="text"
          id="propietario"
          placeholder="Nombre del Propietario"
          class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
          :value="propietario"
          @input="$emit('update:propietario', $event.target.value)"
        />
      </div>

      <div class="mb-5">
        <label for="email" class="block text-gray-700 uppercase font-bold">
          Email
        </label>
        <input
          type="email"
          id="email"
          placeholder="Email del Propietario"
          class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
          :value="email"
          @input="$emit('update:email', $event.target.value)"
        />
      </div>

      <div class="mb-5">
        <label for="date" class="block text-gray-700 uppercase font-bold">
          Alta
        </label>
        <input
          type="date"
          id="date"
          class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
          :value="date"
          @input="$emit('update:date', $event.target.value)"
        />
      </div>

      <div class="mb-5">
        <label for="sintomas" class="block text-gray-700 uppercase font-bold">
          Sintomas
        </label>
        <textarea
          id="sintomas"
          placeholder="Sintomas del paciente"
          class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md h-40"
          :value="sintomas"
          @input="$emit('update:sintomas', $event.target.value)"
        >
        </textarea>
      </div>
      <input
        type="submit"
        :value="editando ? 'Registrar cambios' : 'Registrar Paciente'"
        class="w-full p-3 text-white uppercase font-bold cursor-pointer"
        :class="
          editando
            ? 'bg-violet-500 hover:bg-violet-700 transition-colors'
            : 'bg-indigo-600 hover:bg-indigo-700 transition-colors'
        "
      />

      <Alerta v-if="alerta.mensaje" :alerta="alerta" class="mt-4" />
    </form>
  </div>
</template>

<script setup>
import { computed, reactive } from "vue";
import Alerta from "./Alerta.vue";

const props = defineProps({
  id: {
    type: [String, null],
    required: true,
  },
  nombre: {
    type: String,
    required: true,
  },
  propietario: {
    type: String,
    required: true,
  },
  email: {
    type: String,
    required: true,
  },
  date: {
    type: String,
    required: true,
  },
  sintomas: {
    type: String,
    required: true,
  },
});

const alerta = reactive({
  tipo: "",
  mensaje: "",
});

const emit = defineEmits([
  "update:nombre",
  "update:propietario",
  "update:email",
  "update:date",
  "update:sintomas",
  "guardar-paciente",
]);

const validar = () => {
  if (Object.values(props).includes("")) {
    alerta.mensaje = "Todos los campos son obligatorios";
    alerta.tipo = "error";

    return;
  }
  emit("guardar-paciente");

  alerta.mensaje = "Paciente Almacenado Correctamente";
  alerta.tipo = "exito";

  setTimeout(() => {
    Object.assign(alerta, {
      tipo: "",
      mensaje: "",
    });
  }, 3000);
};

const editando = computed(() => {
  return props.id;
});
</script>
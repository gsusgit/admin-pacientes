<script setup>
import Header from './components/Header.vue'
import Formulario from './components/Formulario.vue'
import { ref, reactive } from 'vue'

const pacientes = ref([])

const paciente = reactive({
  mascota: '',
  propietario: '',
  email: '',
  alta: '',
  sintomas: '',
})

const agregarPaciente = () => {
  pacientes.value.push(paciente)
}

</script>

<template>
  <div class="container mx-auto mt-20">
    <Header />
    <div class="mt-12 md:flex">
      <Formulario
          v-model:mascota="paciente.mascota"
          v-model:propietario="paciente.propietario"
          v-model:email="paciente.email"
          v-model:alta="paciente.alta"
          v-model:sintomas="paciente.sintomas"
          @agregar-paciente="agregarPaciente"
      />
      <div class="md:w-1/2 md:h-screen overflow-y-scroll">
        <h3 class="font-bold text-3xl text-center font-sans">Administra tus pacientes</h3>
        <div v-if="pacientes.length > 0">

        </div>
        <p v-else class="mt-20 text-2xl text-center">No hay pacientes</p>
      </div>
    </div>
  </div>
</template>

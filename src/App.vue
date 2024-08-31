<script setup>
  import { ref, reactive, watch, onMounted } from 'vue'
  import { uid } from "uid";
  import Header from './components/Header.vue'
  import Formulario from './components/Formulario.vue'
  import Paciente from './components/Paciente.vue'

  const pacientes = ref([])

  const guardarLocalStorage = () => {
    localStorage.setItem('pacientes-veterinaria', JSON.stringify(pacientes.value))
  }

  watch(pacientes, () => {
    guardarLocalStorage()
  }, {
    deep: true
  })

  onMounted(() => {
    const storagePacientes = localStorage.getItem('pacientes-veterinaria')
    if (storagePacientes) {
      pacientes.value = JSON.parse(storagePacientes)
    }
  })

  const paciente = reactive({
    id: null,
    mascota: '',
    propietario: '',
    email: '',
    alta: '',
    sintomas: ''
  })

  const agregarPaciente = () => {
    if (paciente.id) {
      const { id } = paciente
      const i = pacientes.value.findIndex((pacienteState) => pacienteState.id === id)
      pacientes.value[i] = {...paciente}
    } else {
      pacientes.value.push({
        ...paciente,
        id: uid(4)
      })
    }
    Object.assign(paciente, {
      id: null,
      mascota: '',
      propietario: '',
      email: '',
      alta: '',
      sintomas: ''
    })
  }

  const editarPaciente = (id) => {
    const pacienteAEditar = pacientes.value.filter(paciente => paciente.id === id)[0]
    Object.assign(paciente, pacienteAEditar)
  }

  const borrarPaciente = (id) => {
    pacientes.value = pacientes.value.filter(paciente => paciente.id !== id)
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
          :id="paciente.id"
      />
      <div class="md:w-1/2 md:h-screen overflow-y-scroll">
        <div v-if="pacientes.length > 0">
          <p class="text-lg mt-5 text-center mb-10">Información de <span class="text-indigo-600 font-bold">Pacientes</span></p>
          <Paciente
            v-for="paciente in pacientes"
            :paciente="paciente"
            @editar-paciente="editarPaciente"
            @borrar-paciente="borrarPaciente"
          />
        </div>
        <p v-else class="mt-20 text-2xl text-center">No hay pacientes</p>
      </div>
    </div>
  </div>
</template>

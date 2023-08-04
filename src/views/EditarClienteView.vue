<script setup>
import { ref, reactive, onMounted } from "vue";
import { FormKit } from "@formkit/vue";
import {useRoute, useRouter} from "vue-router"
import Heading from "../components/UI/Heading.vue";
import RouterLink from "../components/UI/RouterLink.vue";
import ClienteService from '../services/ClienteService';

const route = useRoute()
const router = useRouter()

const cliente = reactive({})

onMounted(() => {
  ClienteService.obtenerCliente(route.params.id)
    .then(response => {
      // Usando Ref
      // cliente.value = response.data
      //Usando reactive
      Object.assign(cliente, response.data)
    })
    .catch(error => console.log)
})

defineProps({
  titulo: {
    type: String,
  },
});

const handleSubmit = (data) => {
  ClienteService.actualizarCliente(route.params.id, data)
    .then(response => {
      router.push({name: 'inicio'})
    })
    .catch(error => console.log)
}

</script>

<template>
  <div>
    <div class="flex justify-end">
      <RouterLink to="inicio">Volver</RouterLink>
    </div>
    <Heading>{{ titulo }}</Heading>

    <div class="mx-auto mt-10 bg-white shadow">
      <div class="mx-auto md:w-2/3 py-20 px-6">
        <FormKit 
          type="form"
          submit-label="Guardar Cambios"
          incomplete-message="No se pudo enviar el formulario, revisa los mensajes de error!"
          @submit="handleSubmit"
        >
          <FormKit
            type="text"
            name="nombre"
            label="Nombre"
            placeholder="Nombre del cliente"
            validation="required"
            :validation-messages="{ required: 'El nombre del cliente es obligatorio' }"
            v-model="cliente.nombre"
          />
          <FormKit
            type="text"
            name="apellido"
            label="Apellido"
            placeholder="Apellido del cliente"
            validation="required"
            :validation-messages="{ required: 'El apellido del cliente es obligatorio' }"
            v-model="cliente.apellido"
          />
          <FormKit
            type="email"
            name="email"
            label="Correo Electrónico"
            placeholder="Correo electrónico del cliente"
            validation="required|email"
            :validation-messages="{ required: 'El correo electrónico del cliente es obligatorio', email: 'Coloca un correo electrónico válido' }"
            v-model="cliente.email"
          />
          <FormKit
            type="text"
            name="telefono"
            label="Teléfono"
            placeholder="Teléfono: XXX-XXX-XXXX"
            validation="*matches:/^[0-9]{3}-[0-9]{3}-[0-9]{4}$/"
            :validation-messages="{ matches: 'El formato no es válido' }"
            v-model="cliente.telefono"
          />

          <FormKit
            type="text"
            name="empresa"
            label="Empresa"
            placeholder="Empresa del cliente"
            v-model="cliente.empresa"
          />

          <FormKit
            type="text"
            name="puesto"
            label="Puesto"
            placeholder="Puesto del cliente"
            v-model="cliente.puesto"
          />

        </FormKit>

      </div>
    </div>
  </div>
</template>

<style>
.formkit-wrapper{
  max-width: 100%;
}
</style>

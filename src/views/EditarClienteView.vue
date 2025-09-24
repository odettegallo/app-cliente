<script setup>
import RouterLink from '@/components/UI/RouterLink.vue';
import Heading from '@/components/UI/Heading.vue';
import { FormKit } from '@formkit/vue';
import ClienteService from '@/services/ClienteService';
import { useRouter, useRoute } from 'vue-router';
import { onMounted, reactive } from 'vue';

const router = useRouter()
const route = useRoute()

const {id} = route.params
console.log(id)

const formData = reactive({
    nombre: '',
    apellido: '',
    email: '',
    telefono: '',
    empresa: '',
    puesto: ''
})

onMounted(() => {
    ClienteService.obtenerCliente(id)
        .then(({data}) => {
            formData.nombre = data.nombre
            formData.apellido = data.apellido
            formData.email = data.email
            formData.telefono = data.telefono
            formData.empresa = data.empresa
            formData.puesto = data.puesto
        })
        .catch((error) => {
            console.log(error)
        })
})
defineProps({
    titulo: {
        type: String
    }
})

const handleSubmit = (data) => {
   
}

</script>

<template>
    <div>
        <div class="flex justify-end">
            <RouterLink to="inicio">
                Volver
            </RouterLink>

        </div>
    </div>

    <Heading>{{ titulo }}</Heading>

    <div class="bg-white shadow rounded-md max-w-3xl mx-auto p-4 mt-10">
        <div class="mx-auto md:w-2/3 py-20 px-6">
            <FormKit type="form" submit-label="Editar Cliente"
                incomplete-message="Por favor rellene todos los campos" @submit="handleSubmit" :value="formData">
                <FormKit type="text" label="Nombre" name="nombre" placeholder="Nombre del Cliente" validation="required"
                    :validation-messages="{ required: 'El Nombre del Cliente es obligatorio' }" 
                    v-model="formData.nombre" />

                <FormKit type="text" label="Apellido" name="apellido" placeholder="Apellido del Cliente"
                    validation="required"
                    :validation-messages="{ required: 'El Apellido del Cliente es obligatorio' }" 
                    v-model="formData.apellido" />

                <FormKit type="email" label="Email" name="email" placeholder="Email del Cliente"
                    validation="required | email"
                    :validation-messages="{ required: 'El Email del Cliente es obligatorio', email: 'El Email no es válido' }" 
                    v-model="formData.email" />

                <FormKit type="text" label="Telefono" name="telefono" placeholder="Telefono: XXX-XXX-XXX"
                    validation="?matches:/^[0-9]{9}$/"
                    :validation-messages="{ matches: 'El Telefono no es válido' }"
                    v-model="formData.telefono"
                     />

                <FormKit type="text" label="Empresa" name="empresa" placeholder="Empresa del Cliente"
                    v-model="formData.empresa" />
                <FormKit type="text" label="Puesto" name="puesto" placeholder="Puesto del Cliente"
                    v-model="formData.puesto" />


            </FormKit>
        </div>


    </div>
</template>

<style>
.formkit-outer {
    max-width: 100%;
}
</style>
<script setup>
    import { reactive, computed } from "vue";
    import Alerta from "./Alerta.vue";

    const alerta = reactive({
        tipo: '',
        mensaje: '',
    })

    const emeit = defineEmits(['update:nombre','update:propietario', 'update:email', 'update:fecha-alta', 'update:sintomas','guardar-paciente']);

    const validar = () =>{
        if(Object.values(props).includes('')){
            alerta.mensaje = "Todos los campos son necesarios";
            alerta.tipo = "error";
            return;
        }
        emeit('guardar-paciente')
        alerta.mensaje = "Paciente Guardado Correctamente";
        alerta.tipo = "exito";

        setTimeout(() => {
            Object.assign(alerta,{
                tipo: '',
                mensaje: '',
            })
        }, 3000);
    }

    const editando = computed (() => {
        return props.id;
    }) 

    const props = defineProps({
        id:{
            type: [String, null],
            required: true,
        },
        nombre:{
            type: String,
            required: true,
        },
        propietario:{
            type: String,
            required: true,
        },
        email:{
            type: String,
            required: true,
        },
        fechaAlta:{
            type: String,
            required: true,
        },
        sintomas:{
            type: String,
            required: true,
        }
    })
</script>

<template>
    <div class="md:w-1/2">
        <h2 class="font-black text-3xl text-center">Seguimiento Pacientes</h2>
        <p class="text-lg mt-5 text-center mb-10">
            Añade Pacientes y <span class="text-indigo-600 font-bold">Administralos</span>
        </p>
        <Alerta v-if="alerta.mensaje" :alerta="alerta"/>
        <form class="bg-white shadow-md rounded-lg py-10 px-5 mb-10" @submit.prevent="validar">
            <div class="mb-5">
                <label for="mascota" class="block text-gray-700 uppercase font-bold">Nombre mascota</label>
                <input id="mascota" type="text" placeholder="Nombre de la mascota" class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md" @input="$emit('update:nombre', $event.target.value)" :value="nombre">
            </div>
            <div class="mb-5">
                <label for="propietario" class="block text-gray-700 uppercase font-bold">Nombre propietario</label>
                <input id="propietario" type="text" placeholder="Nombre del propietario" class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md" @input="$emit('update:propietario', $event.target.value)" :value="propietario">
            </div>
            <div class="mb-5">
                <label for="email" class="block text-gray-700 uppercase font-bold">Email</label>
                <input id="email" type="email" placeholder="Email del propietario" class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md" @input="$emit('update:email', $event.target.value)" :value="email">
            </div>
            <div class="mb-5">
                <label for="alta" class="block text-gray-700 uppercase font-bold">Fecha de alta</label>
                <input id="alta" type="date" class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md" @input="$emit('update:fecha-alta', $event.target.value)" :value="fechaAlta">
            </div>
            <div class="mb-5">
                <label for="sintomas" class="block text-gray-700 uppercase font-bold">Sintomas</label>
                <textarea id="sintomas" placeholder="Describe los sintomas" class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md h-40"  @input="$emit('update:sintomas', $event.target.value)" :value="sintomas"/>
            </div>
            <input type="submit" class="bg-indigo-600 w-full p-3 text-white uppercase font-bold hover:bg-indigo-700 cursor-pointer transition-colors" :value="[editando ? 'Guardar Cambios' : 'Registrar Paciente']">
        </form>
    </div>
</template>
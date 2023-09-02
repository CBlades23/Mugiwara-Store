<script setup>

    import { ref } from 'vue'

    const props = defineProps({
        figura: {
            type: Object,
            required: true
        }
    })

    defineEmits(['agregar-carrito'])

</script>

<template>
    <div class="col-md-6 col-lg-4 my-4 row align-items-center">
        <div class="col-4">
            <img 
                class="img-fluid" 
                v-bind:src="'/img/' + figura.imagen + '.png'" 
                v-bind:alt="'imagen ' + figura.nombre"
                loading="lazy"
            >
        </div>

        <div class="col-8">
            <h3 class="text-black fs-4 fw-bold text-uppercase"> {{ figura.nombre }} </h3>
            <p class="figura-descripcion">{{ figura.descripcion }}</p>
            <p class="fw-black text-primary fs-3">${{ figura.precio }}</p>
            <button 
                type="button" 
                class="btn btn-dark w-100" 
                @click="$emit('agregar-carrito', figura)"
            >Agregar al Carrito</button>
        </div>

    </div><!-- FIN figura -->
</template>
<script setup>
    import { ref, reactive, onMounted, watch } from 'vue'
    import { db } from './data/figuras'
    import Figura from './components/Figura.vue'
    import Header from './components/Header.vue'
    import Footer from './components/Footer.vue'

    const figuras = ref([])
    const carrito = ref([])
    const figura = ref({})

    watch(carrito, () => {
        guardarLocalStorage()
    }, {
        deep: true
    })

    onMounted(() => {
        figuras.value = db;
        figura.value = db[10];

        const carritoStorage = localStorage.getItem('carrito')
        if (carritoStorage) {
            carrito.value = JSON.parse(carritoStorage)
        }
    });

    const guardarLocalStorage = () => {
        localStorage.setItem('carrito', JSON.stringify(carrito.value))
    }

    const agregarCarrito = (figura) => {

        const existeCarrito = carrito.value.findIndex(producto => producto.id === figura.id)

        if (existeCarrito >= 0) {
            carrito.value[existeCarrito].cantidad++
        } else {
            figura.cantidad = 1;
            carrito.value.push(figura);
        }
    }

    const decrementarCantidad = (id) => {
        const index = carrito.value.findIndex(producto => producto.id === id)
        if (carrito.value[index].cantidad <= 1) return
        carrito.value[index].cantidad--
    }
    
    const incrementarCantidad = (id) => {
        const index = carrito.value.findIndex(producto => producto.id === id)
        if (carrito.value[index].cantidad >= 5) return
        carrito.value[index].cantidad++
    }

    const eliminarProducto = (id) => {
        carrito.value = carrito.value.filter(producto => producto.id !== id)
    }

    const vaciarCarrito = (figura) => {
        carrito.value = carrito.value.filter(producto => producto.id === figura.id)
    }

</script>

<template>
    <!-- IMPORTAR HEADER -->
    <Header 
        :carrito="carrito" 
        :figura="figura" 
        @decrementar-cantidad="decrementarCantidad"
        @incrementar-cantidad="incrementarCantidad"
        @agregar-carrito="agregarCarrito"
        @vaciar-carrito="vaciarCarrito"
        @eliminar-producto="eliminarProducto"
    />

    <main class="container-xl mt-5">
        <h2 class="text-center">Nuestra Colección</h2>

        <div class="row mt-5">

            <!-- COMPONENTE -->
            <Figura 
                v-for="figura in figuras" 
                :key="figura.id"
                :figura="figura" 
                @agregar-carrito="agregarCarrito" 
            />

        </div>
    </main>

    <!-- IMPORTAR FOOTER -->
    <Footer />
</template>

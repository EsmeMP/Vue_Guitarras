<script setup>
import { ref, reactive, onMounted } from 'vue'
import { db } from './data/guitarras'
import Footer from './components/Footer.vue';
import Guitarra from './components/Guitarra.vue';
import Header from './components/Header.vue';

const guitarras = ref(db)
const carrito = ref([])

function agregarCarrito(guitarra){
    // guitarra.cantidad = 1
    // carrito.value.push(guitarra)

    const idGuitarra = carrito.value.findIndex(g => g.id === guitarra.id)
    if(idGuitarra === -1){
        guitarra.cantidad =1
        carrito.value.push(guitarra)
    }else
    carrito.value[idGuitarra].cantidad++
}

function agregaUno(id){
    const idGuitarra = carrito.value.findIndex(g => g.id === id)
    carrito.value[idGuitarra].cantidad++
}

function quitaUno(id){
    const idGuitarra = carrito.value.findIndex(g => g.id === id)
    carrito.value[idGuitarra].cantidad--
}
</script>

<template>
    <Header :carrito="carrito"
    @agrega-uno ="agregaUno" 
    @quita-uno ="quitaUno" />
    <main class="container-xl mt-5">
        <h2 class="text-center">Nuestra Colección</h2>
        <div class="row mt-5">
            <Guitarra 
            v-for="guitarra in guitarras"
            :guitarra="guitarra"
            @agregar-carrito="agregarCarrito"
            />
        </div>

    </main>
    <Footer />
</template>
<style scoped>
</style>
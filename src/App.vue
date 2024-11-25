<script setup>
import { ref, reactive, onMounted } from 'vue'
import { db } from './data/guitarras'
import Footer from './components/Footer.vue';
import Guitarra from './components/Guitarra.vue';
import Header from './components/Header.vue';
import { computed } from 'vue'

const guitarras = ref(db)
const carrito = ref([])
const guitarra = ref(db[3])

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
    // carrito.value[idGuitarra].cantidad--
    if (carrito.value[idGuitarra].cantidad  > 1)
    carrito.value[idGuitarra].cantidad--
}

function quitarCarrito(id){
    carrito.value = carrito.value.filter(g => g.id !== id)
}

function vaciarCarrito(){
    carrito.value=[]
}

const total = computed(() => {
    let total = 0
    carrito.value.forEach(g =>{
        total += g.cantidad * g.precio
    })
    return total
})
</script>

<template>
    <Header :carrito="carrito"
    :total="total"
    :guitarra="guitarra"
    @agregar-carrito="agregarCarrito"
    @agrega-uno ="agregaUno" 
    @quita-uno ="quitaUno"
    @quitar-carrito ="quitarCarrito"
    @vacia-carrito ="vaciarCarrito"
     />
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
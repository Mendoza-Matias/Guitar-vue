<script setup>
/*Activo composition API*/
/*Codigo de Javascript */
import { ref, onMounted,watch} from 'vue';
import { db } from '../src/data/guitarras'
import Card from '../src/components/Cards.vue'
import Header from '../src/components/Header.vue'
import Footer from '../src/components/Footer.vue'

const guitarras = ref([]); //Uso de ref
const carrito = ref([]);
const guitarra = ref({});

//Usando watch
//Detecta cambios en el state y acciona una funcion
watch(carrito,()=>{ 
    guardarLocalStorage()
},{
    deep:true //Revisa cada uno de los elementos
})

// Cuando el componente esta listo carga la información
onMounted(() => {
    guitarras.value = db;
    guitarra.value = db[3];

    const carritoStorage = localStorage.getItem('carrito')

    if(carritoStorage){
        carrito.value = JSON.parse(carritoStorage);
    }

})

const guardarLocalStorage = () =>{
    localStorage.setItem('carrito',JSON.stringify(carrito.value)); //Almaceno la informacion al localStorage
}


//Funciones

const agregarCarrito = (guitarra) => { /*Le paso a mi funcion los datos de mi guitarra */

    const existeCarrito = carrito.value.findIndex(producto => producto.id === guitarra.id);

    if (existeCarrito >= 0) {
        carrito.value[existeCarrito].cantidad++ //Mofico el state
    } else {
        guitarra.cantidad = 1;
        /*Añado un valor*/
        carrito.value.push(guitarra); /*Modifico el carrito*/
    }

    // guardarLocalStorage() //Cuando yo tenga algo lo guardo en el storage
}
const vaciarCarrito = () => {
    carrito.value = [];
}
const incrementarCantidad = (id) => {
    const indice = carrito.value.findIndex(producto => producto.id === id);
    if(carrito.value[indice].cantidad >= 5) return;
    carrito.value[indice].cantidad++;
}
const decrementarCantidad = (id) => {
    const indice = carrito.value.findIndex(producto => producto.id === id);
    if(carrito.value[indice].cantidad <= 1) return;
    carrito.value[indice].cantidad--;
    
}
const eliminarCompra = (id) => {
    carrito.value = carrito.value.filter(producto => producto.id !== id);
    
}

</script>

<template>
    <!--Codigo Html-->
    <Header v-bind:carrito="carrito" 
    v-bind:guitarra="guitarra"
    @vacias-carrito="vaciarCarrito" 
    @incrementar-cantidad="incrementarCantidad"
    @decrementar-cantidad="decrementarCantidad" 
    @eliminar-compra="eliminarCompra"
    @agregar-carrito="agregarCarrito"/>

    <main class="container-xl mt-5">
        <h2 class="text-center">Nuestra Colección</h2>

        <div class="row mt-5">
            <Card v-for="guitarra in guitarras" v-bind:guitarra="guitarra" @agregar-carrito="agregarCarrito" />
            <!-- 
                Vuelvo mi HTML dinamico
                De esta forma paso un props (v-bind:nombrePropiedad="valor")-->
        </div>
    </main>

    <Footer />
</template>

<style scoped>
/* Codigo Css */
</style>

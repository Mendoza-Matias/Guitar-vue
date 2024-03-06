<script setup>
import { computed } from 'vue';

const props = defineProps({
    // defino en nombre de mi prop
    carrito: {
        type: Array,
        required: true
    },
    guitarra:{
        type:Object,
        required:true
    }
})

/*Custome events */
defineEmits([
'vacias-carrito',
'incrementar-cantidad',
'decrementar-cantidad',
'eliminar-compra',
'agregar-carrito'])

const carritoEstaVacio = (carritoData) => {
    return carritoData.length === 0;
}

//Aplicando computed para obtener el total
const totalPagar = computed(() => {
    return props.carrito.reduce((total,producto)=> total + (producto.cantidad * producto.precio),0);
    /*Inicializa en 0 total y luego se realiza la operacion y se le asigna a total */
})

</script>
<template>
    <header class="py-5 header">
        <div class="container-xl">
            <div class="row justify-content-center justify-content-md-between">
                <div class="col-8 col-md-3">
                    <a href="index.html">
                        <img class="img-fluid" src="/img/logo.svg" alt="imagen logo">
                    </a>
                </div>
                <nav class="col-md-6 a mt-5 d-flex align-items-start justify-content-end">
                    <div class="carrito">
                        <img class="img-fluid" src="/img/carrito.png" alt="imagen carrito" />

                        <div id="carrito" class="bg-white p-3">
                            <p v-if="carritoEstaVacio(carrito)" class="text-center">El carrito esta vacio</p>
                            <div v-else> <!--Sino esta vacio muestra esto , debe haber antes un v-if -->
                                <table class="w-100 table">
                                    <thead>
                                        <tr>
                                            <th>Imagen</th>
                                            <th>Nombre</th>
                                            <th>Precio</th>
                                            <th>Cantidad</th>
                                            <th></th>
                                        </tr>
                                    </thead>

                                    <tbody v-for="carritoItem in carrito">
                                        <tr>
                                            <td>
                                                <img class="img-fluid" v-bind:src="'img/' + carritoItem.imagen + '.jpg'"
                                                    alt="imagen guitarra">
                                            </td>
                                            <td>{{ carritoItem.nombre }}</td>
                                            <td class="fw-bold">
                                                {{ carritoItem.precio }}
                                            </td>
                                            <td class="flex align-items-start gap-4">
                                                <button v-on:click="$emit('decrementar-cantidad',carritoItem.id)" type="button" class="btn btn-dark">
                                                    -
                                                </button>
                                                {{ carritoItem.cantidad }}
                                                <button v-on:click="$emit('incrementar-cantidad',carritoItem.id)" type="button" class="btn btn-dark">
                                                    +
                                                </button>
                                            </td>
                                            <td>
                                                <button v-on:click="$emit('eliminar-compra',carritoItem.id)" class="btn btn-danger" type="button">
                                                    X
                                                </button>
                                            </td>
                                        </tr>
                                    </tbody>
                                </table>


                                <p class="text-end">Total pagar: <span class="fw-bold">{{ totalPagar }}</span></p>
                                <button v-on:click="$emit('vacias-carrito')" class="btn btn-dark w-100 mt-3 p-2">Vaciar
                                    Carrito</button>
                            </div>
                        </div>
                    </div>
                </nav>
            </div><!--.row-->

            <div class="row mt-5">
                <div class="col-md-6 text-center text-md-start pt-5">
                    <h1 class="display-2 fw-bold">Modelo{{ guitarra.nombre }}</h1>
                    <p class="mt-5 fs-5 text-white">{{ guitarra.descripcion }}</p>
                    <p class="text-primary fs-1 fw-black">{{ guitarra.precio }}</p>
                    <button v-on:click="$emit('agregar-carrito',guitarra)" type="button" 
                    class="btn fs-4 bg-primary text-white py-2 px-5">
                    Agregar al Carrito</button>
                </div>
            </div>
        </div>

        <img class="header-guitarra" src="/img/header_guitarra.png" alt="imagen header">
    </header>
</template>

<style></style>
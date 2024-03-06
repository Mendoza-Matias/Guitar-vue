<script setup> /*No olvidar de agregar el setup */
    import{ref} from 'vue'
    //Props
    const props = defineProps({
        /*Defino mi props 
        Su tipo */
        guitarra:{
            type:Object,
            required:true
        }
    })

    defineEmits(['agregar-carrito'])


    /*Defino mis eventos */

    const num = ref(0); //Inicializo 

    /*Method handler*/
    

</script>
<template>
     <div class="col-md-6 col-lg-4 my-4 row align-items-center">
                <div class="col-4">
                    <img class="img-fluid" 
                    v-bind:src="'img/'+guitarra.imagen+'.jpg'" 
                    v-bind:alt="'guitarra imagen' + guitarra.nombre">
                </div>
                <div class="col-8">
                    <h3 class="text-black fs-4 fw-bold text-uppercase">{{ guitarra.nombre }}</h3>
                    <p>{{ num }}</p>
                    <p>{{ guitarra.descripcion}}</p>
                    <p class="fw-black text-primary fs-3">{{ guitarra.precio }}</p>
                    <!-- Con v-on: añado un evento / @click-->
                    <button 
                        v-on:click="$emit('agregar-carrito', guitarra)" 
                        type="button"
                        class="btn btn-dark w-100 "
                    >Agregar al Carrito</button>

                    <!--Emito el evento y paso el nombre del emisor , la informacion que quiero transferir -->

                </div>
            </div><!-- FIN GUITARRA -->
</template>

<style setup>
    
</style>
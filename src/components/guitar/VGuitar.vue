<script setup>
   const props = defineProps({
        guitar:{
            type: Object,
            require: true
        },
   });
   defineEmits(['add-to-cart'])

</script>
<template>
    <div class="col-md-6 col-lg-4     my-4 row align-items-center">
        <div class="col-4">
            <img class="img-fluid" 
                 :src="'/img/'+guitar.img+'.jpg'" 
                 :alt="'imagen guitarra '+guitar.name">
        </div>
        <div class="col-8">
            <h3 class="text-black fs-4 fw-bold text-uppercase">{{guitar.name}}</h3>
            <p>{{guitar.description}}</p>
            <p class="fw-black text-primary fs-3">${{ guitar.price.toFixed(2) }}</p>
            <button 
                @click="$emit('add-to-cart', guitar)"
                type="button"
                class="btn btn-dark w-100 "
            >Agregar al Carrito</button>
        </div>
    </div><!-- FIN GUITARRA -->
</template>

<style>

</style>
<script setup>
    import { ref, reactive, onMounted, watch } from "vue";
    import {db} from "./data/guitars";
    import VGuitar from "./components/guitar/VGuitar.vue";
    import VHeader from "./components/guitar/VHeader.vue";
    import VFooter from "./components/guitar/VFooter.vue";

    const guitars = ref([]);
    const guitar = ref({});
    const cart = ref([]);

    watch(cart, ()=>{
        saveLocalStorage();
    },{
        deep: true
    })

    onMounted(() =>{
        guitars.value = db;
        guitar.value = db[3];
        const cartStorage = localStorage.getItem('cart');

        if(cartStorage){
            cart.value = JSON.parse(cartStorage);
        }
    })

    const saveLocalStorage = () =>{
        localStorage.setItem('cart', JSON.stringify(cart.value));
    }

    const addToCart = (guitar) =>{
        const existCart = cart.value.findIndex(product => product.id === guitar.id);
        if (existCart >= 0) {
            cart.value[existCart].quantity++;
            alert("Se agrego un producto..");

        }else{
            guitar.quantity = 1;
            cart.value.push(guitar);
            alert("Se agrego un producto..");
        }
    }

    const addToCartHeader = (guitar) =>{
        guitar.quantity = 1;
        cart.value.push(guitar);
        
    }

    const decreaseQuantity = (id) =>{
        const index = cart.value.findIndex(product => product.id === id);
        if ( cart.value[index].quantity <= 1) return
        cart.value[index].quantity--

        
    }
    const increaseQuantity = (id) =>{
        const index = cart.value.findIndex(product => product.id === id);
        cart.value[index].quantity++

    }

    const deleteProduct = (id) =>{
        cart.value = cart.value.filter(product => product.id !== id)
    }

    const voidCart = () =>{
        cart.value = [];
    }
</script>

<template>
    <VHeader
        :guitar="guitar"
        :cart="cart"
        @decrease-quantity="decreaseQuantity"
        @increase-quantity="increaseQuantity"
        @add-to-cart-header="addToCartHeader"
        @delete-product="deleteProduct"
        @void-cart="voidCart"
    />
        <main class="container-xl mt-5">
            <h2 class="text-center">Nuestra Colección</h2>
            <div class="row mt-5">
            <VGuitar 
                v-for="guitar in guitars" :key="guitar.id"
                @add-to-cart="addToCart"
                :guitar="guitar"
            />
            </div>
        </main>
    <VFooter/>
</template>

<style lang="scss" scoped>

</style>

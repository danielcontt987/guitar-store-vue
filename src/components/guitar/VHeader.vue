<script setup>
    import { computed } from "vue";

    const props = defineProps({
        cart:{
            type: Array,
            require: true,
        },
        guitar: {
            type: Object,
            require: true,
        }
    })
    defineEmits(['decrease-quantity','increase-quantity', 'add-to-cart-header','delete-product', 'void-cart'])
    const total = computed(()=>{
        return props.cart.reduce((total, car) => total + (car.quantity*car.price),0)
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
                    <div 
                        class="carrito"
                    >
                        <img class="img-fluid" src="/img/carrito.png" alt="imagen carrito" style=" position: relative;"/>
                        <span id="cart_menu_num" data-action="cart-can" class="badge rounded-circle">{{cart.length}}</span>

                        <div id="carrito" class="bg-white p-3">
                            <p v-if="cart.length == 0" class="text-center">El carrito esta vacio</p>
                            <table v-else class="w-100 table">
                                <thead>
                                    <tr>
                                        <th>Imagen</th>
                                        <th>Nombre</th>
                                        <th>Precio</th>
                                        <th>Cantidad</th>
                                        <th></th>
                                    </tr>
                                </thead>
                                <tbody v-for="car in cart" :key="car.id">
                                    <tr>
                                        <td>
                                            <img class="img-fluid" :src="'/img/'+car.img+'.jpg'" alt="imagen guitarra">
                                        </td>
                                        <td>{{car.name}}</td>
                                        <td class="fw-bold">
                                                ${{ car.price.toFixed(2) }}
                                        </td>
                                        <td class="flex align-items-start gap-4">
                                            <button
                                                type="button"
                                                class="btn btn-dark"
                                                @click="$emit('decrease-quantity', car.id)"
                                            >
                                                -
                                            </button>
                                            {{car.quantity}}
                                            <button
                                                type="button"
                                                class="btn btn-dark"
                                                @click="$emit('increase-quantity', car.id)"
                                            >
                                                +
                                            </button>
                                        </td>
                                        <td>
                                            <button
                                                @click="$emit('delete-product', car.id)"
                                                class="btn btn-danger"
                                                type="button"
                                            >
                                                X
                                            </button>
                                        </td>
                                    </tr>
                                </tbody>
                            </table>

                            <p v-if="cart.length > 0" class="text-end">Total pagar: <span class="fw-bold">${{total.toFixed(2)}}</span></p>
                            <button v-if="cart.length > 0" class="btn btn-dark w-100 mt-3 p-2" @click="$emit('void-cart')">Vaciar Carrito</button>
                        </div>
                    </div>
                </nav>
            </div><!--.row-->

            <div class="row mt-5">
                <div class="col-md-6 text-center text-md-start pt-5">
                    <h1 class="display-2 fw-bold">Modelo {{ guitar.name }}</h1>
                    <p class="mt-5 fs-5 text-white">{{guitar.description}}</p>
                    <p class="text-primary fs-1 fw-black">${{guitar.price}}</p>
                    <button 
                        @click="$emit('add-to-cart-header', guitar)"
                        type="button"
                        class="btn fs-4 bg-primary text-white py-2 px-5"
                    >Agregar al Carrito</button>
                </div>
            </div>
        </div>

        <img class="header-guitarra" src="/img/header_guitarra.png" alt="imagen header">
    </header>
</template>
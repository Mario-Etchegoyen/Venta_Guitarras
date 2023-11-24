<script setup>
import { computed } from 'vue';
const props = defineProps({
  carrito: {
    type: Array,
    required: true,
  },
  guitarra: {
    type: Object,
    required: true,
  }
});

defineEmits([
  "decrementar-cantidad",
  "incrementar-cantidad",
  "agregar-carrito", "eliminar-producto", "vaciar-carrito"])

const totalPagar = computed(() => {
  return props.carrito.reduce((total, producto) => total + (producto.cantidad * producto.precio), 0)
})
</script>
<template>
  <header class="py-5 header">
    <div class="container-xl">
      <div class="row justify-content-center justify-content-md-between">
        <div class="col-8 col-md-3">
          <a href="index.html">
            <img class="ii" src="public/img/logo.svg" alt="imagen logo" />
          </a>
        </div>
        <nav class="col-md-6 a mt-5 d-flex align-items-start justify-content-end">
          <div class="carrito">
            <img class="img-carrito" src="public/img/carrito.png" alt="imagen carrito" />
            <div id="carrito" class="bg-gray p-3 --bs-border-radius-lg">
              <p v-if="carrito.length === 0" class="text-center m-0 fw-bold">
                El carrito esta vacio
              </p>
              <!-- desde aca -->

              <!-- hasta acá -->

              <!-- modelo a copiar -->

              <div v-else>
                <table class="w-100 table">
                  <thead>
                    <tr>
                      <th>Imagen</th>
                      <th>Nombre</th>
                      <th>Precio</th>
                      <th>Cantidad</th>
                      <th>Subtotal</th>
                      <th></th>
                    </tr>
                  </thead>

                  <tbody class="popup-container ">
                    <tr v-for="producto in carrito" :key="producto.id" class="">
                      <td>
                        <img class="img-fluid" :src="`./imagen_header/${producto.imagen}.png`"
                          v-bind:alt="`Imagen guitarra ${producto.nombre}`" />
                      </td>
                      <td>
                        {{ producto.nombre }}
                      </td>
                      <td class="fw-bold">${{ producto.precio }}</td>
                      <td class="flex align-items-start gap-4">
                        <button type="button" class=" btn-darko" @click="$emit('decrementar-cantidad', producto.id)">
                          -
                        </button>
                        {{ producto.cantidad }}
                        <button type="button" class="btn-darko" @click="$emit('incrementar-cantidad', producto.id)">
                          +
                        </button>
                      </td>
                      <td>$ {{ Intl.NumberFormat("de-DE").format(producto.precio * producto.cantidad) }}</td>
                      <td>
                        <button class="btn-eliminar " type="button" @click="$emit('eliminar-producto', producto.id)">
                          X
                        </button>
                      </td>
                    </tr>
                  </tbody>
                </table>
                <p class="text-end fondo-carrito">
                  Total a pagar: <span class="fw-bold">$ {{ Intl.NumberFormat("de-DE").format(totalPagar) }}
                  </span>
                </p>
                <button class="btn btn-dark w-100 mt-3 p-2" @click="$emit('')">
                    Pagar
                  </button>
                <button class="btn btn-dark w-100 mt-3 p-2" @click="$emit('vaciar-carrito')">
                  Vaciar Carrito
                </button>
              </div>
            </div>
          </div>
        </nav>
      </div>
      <div class="row mt-5">
        <div class="col-md-6 text-center text-md-start pt-5">
          <h3 class="text-center text-white">Guitarra de la Semana</h3>
          <hr>
          <h1 class="display-2 fw-bold guitarra-semana">Modelo {{ guitarra.nombre }}</h1>
          <p class="mt-5 fs-5 text-white">{{ guitarra.descripcion }}</p>
          <p class="text-primary fs-1 fw-black text-end">$ {{ Intl.NumberFormat("de-DE").format(guitarra.precio) }}</p>
          <div class="text-center">
            <button type="button" class="btn fs-4 bg-primary text-white py-2 px-5 "
              @click="$emit('agregar-carrito', guitarra)">
              Agregar al Carrito
            </button>
          </div>
        </div>
      </div>
    </div>

    <img class="header-guitarra" :src="`./imagen_header/${guitarra.imagen}.png`" alt="imagen header" />
  </header>
</template>

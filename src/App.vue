<script setup>
// al escribir setup estoy utilizando COMPOSITION API (no options API)
import { ref, reactive, onMounted, watch } from "vue";
import { db } from "./data/guitarras.js"; // escribo db porque es el nombre de la variable que estoy exportando en guitarras.js con js o sin js?
import Guitarra from "./components/Guitarra.vue";
import Header from "./components/Header.vue";
import Footer from "./components/Footer.vue";

// con REACTIVE (datos agrupados OBJETO)
// const state = reactive({
//   guitarras: db
// })




const guitarras = ref([]) // const guitarras = ref([]) al momento que import onMounted from vue
const carrito = ref([]) // establezco carrito como un arreglo vacio
const guitarra = ref({})


const resultado = db.findIndex(tech => tech.id === idGuitarraOferta)



// GUITARRA DE LA SEMANA ------------------------------------------------------------------------------

const idGuitarraOferta = 9 // cambiando el codigo de 1 a 12 establece GUITARRA DE LA SEMANA
const cantidadMaxVenta = 18, cantidadMinVenta = 0 // establezco max y min unidades de venta

// ----------------------------------------------------------------------------------------------------






watch(carrito, () => {
  guardarLocalStorage()
}, {
  deep: true
})




onMounted(() => {
  guitarras.value = db;
  guitarra.value = db[resultado] // resultado es la posicion GUITARRA DE LA SEMANA

  const carritoStorage = localStorage.getItem("carrito")
  if (carritoStorage) {
    carrito.value = JSON.parse(carritoStorage) // con JSON.parse() convierto el string en un arreglo
  }
});

const guardarLocalStorage = () => {
  localStorage.setItem('carrito', JSON.stringify(carrito.value)) // convierto el arreglo en string
}


//console.log(guitarras.value);

const agregarCarrito = (guitarra) => {
  const existeCarrito = carrito.value.findIndex(
    (producto) => producto.id === guitarra.id,
  );

  if (existeCarrito >= 0) {
    carrito.value[existeCarrito].cantidad++;
  } else {
    guitarra.cantidad = 1;
    carrito.value.push(guitarra); // .push modifica el arreglo original agregando al final una nueva guitarra
  }

};

const decrementarCantidad = (id) => {
  const index = carrito.value.findIndex((producto) => producto.id === id);
  if (carrito.value[index].cantidad <= cantidadMinVenta) return;
  carrito.value[index].cantidad--;

};

const incrementarCantidad = (id) => {
  const index = carrito.value.findIndex((producto) => producto.id === id);
  if (carrito.value[index].cantidad >= cantidadMaxVenta) return;
  carrito.value[index].cantidad++;

};

const eliminarProducto = (id) => {
  carrito.value = carrito.value.filter(producto => producto.id !== id)

}

const vaciarCarrito = () => {
  carrito.value = []

}

</script>
<!-- ------------------------------------------------------------------------------------------------------------------------------------------ -->
<template>
  
  <Header :carrito="carrito" :guitarra="guitarra" @decrementar-cantidad="decrementarCantidad"
    @incrementar-cantidad="incrementarCantidad" @agregar-carrito="agregarCarrito" @eliminar-producto="eliminarProducto"
    @vaciar-carrito="vaciarCarrito">
  </Header>

  <main class="container-xl mt-5">
    <h2 class="text-center rotulo-coleccion">Nuestra Colección</h2>

    <div class="row mt-5 alineando-guitarras">
      <!-- aca iba lineas de una guitarra -->
      <Guitarra v-for="guitarra in guitarras" :guitarra="guitarra" @agregar-carrito="agregarCarrito">
      </Guitarra>
    </div>
  </main>

  <Footer></Footer>
</template>

<!-- ------------------------------------------------------------------------------------------------------------------------------------------ -->

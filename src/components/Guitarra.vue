<script setup>

const props = defineProps({
  guitarra: {
    type: Object,
    required: true,
  },
});

defineEmits(['agregar-carrito'])

// const incrementar = () => {// method  handler
//   numero.value++;
// };
</script>

<template>
  <!-- --------------------------------------------------- INICIO GUITARRA ------------------------------------------------------- -->
  <div class="col-md-6 col-lg-4 my-4 row align-items-center guitarra-elegida">
    <div class="col-4">
      <img class="img-fluid " v-bind:src="`./imagen_header/${guitarra.imagen}.png`"
        v-bind:alt="`Imagen guitarra: ${guitarra.nombre}`" />
    </div>
    <!-- OK nombre descripcion y precio -->
    <div class="col-8 text-center">
      <h3 class=" fs-4 fw-bold text-uppercase">
        {{ guitarra.nombre }}
      </h3>
      <p>{{ guitarra.descripcion }}</p>
      <p class="fw-black text-primary fs-5 text-center">Precio $ {{
        Intl.NumberFormat("de-DE").format(guitarra.precio) }}</p>
      <button type="button" class="btn btn-dark w-100" v-on:click="$emit('agregar-carrito', guitarra)">
        Agregar al carrito
      </button>
    </div>
  </div>


  <!-- ------------------------------------------------------- FIN GUITARRA ----------------------------------------------------------- -->
</template>

<style lang="scss" scoped></style>

<script setup>

  const props = defineProps({
    product: Object,
    required: true,
  })

  defineEmits(['add-to-cart'])

</script>

<template>
  <div class="col-md-6 col-lg-4 my-4 row align-items-center">
    <div class="col-4">
      <img class="img-fluid" :src="'/img/' + product.imagen + '.jpg'" :alt="'imagen ' + product.nombre" />
    </div>
    <div class="col-8">
      <h3 class="text-black fs-4 fw-bold text-uppercase">{{ product.nombre }}</h3>
      <p>
        {{ product.descripcion }}
      </p>
      <p class="fw-black text-primary fs-3">$ {{ product.precio }}</p>
      <button 
        type="button" 
        class="btn btn-dark w-100"
        @click="$emit('add-to-cart', product)"
      >Add to cart</button>
    </div>
  </div>
</template>

<style lang="scss" scoped></style>

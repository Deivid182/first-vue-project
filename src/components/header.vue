<script setup>
  import { computed } from 'vue';

  const props = defineProps({
    cart: {
      type: Array,
      required: true,
    },
    productShowCase: {
      type: Object,
      required: true,
    }
  });

  defineEmits(['increment', 'decrement', 'remove-from-cart', 'add-to-cart', 'empty-cart']);

  const total = computed(() => {
    return props.cart.reduce((total, product) => {
      return total + product.precio * product.quantity
    }, 0);
  })

</script>
<template>
  <header class="py-5 header">
    <div class="container-xl">
      <div class="row justify-content-center justify-content-md-between">
        <div class="col-8 col-md-3">
          <a href="index.html">
            <img class="img-fluid" src="/img/logo.svg" alt="image logo" />
          </a>
        </div>
        <nav
          class="col-md-6 a mt-5 d-flex align-items-start justify-content-end"
        >
          <div class="cart">
            <img class="img-fluid" src="/img/cart.png" alt="image cart" />

            <div id="cart" class="bg-white p-3">
              <p v-if="cart.length == 0" class="text-center">Empty Cart</p>
              <div v-else>
                <table class="w-100 table">
                  <thead>
                    <tr>
                      <th>Image</th>
                      <th>Name</th>
                      <th>Price</th>
                      <th>Quantity</th>
                      <th></th>
                    </tr>
                  </thead>
                  <tbody>
                    <tr v-for="product in cart" :key="product.id">
                      <td>
                        <img
                          class="img-fluid"
                          :src="'/img/' + product.imagen + '.jpg'"
                          :alt="'imagen ' + product.nombre"
                        />
                      </td>
                      <td>{{ product.nombre }}</td>
                      <td class="fw-bold">{{ product.precio }}</td>
                      <td class="flex align-items-start gap-4">
                        <button
                          @click="$emit('decrement', product.id)"
                          type="button"
                          class="btn btn-dark"
                        >
                          -
                        </button>
                        <span>{{ product.quantity }}</span>
                        <button
                          @click="$emit('increment', product.id)"
                          type="button"
                          class="btn btn-dark"
                        >
                          +
                        </button>
                      </td>
                      <td>
                        <button 
                          @click="$emit('remove-from-cart', product.id)"
                          class="btn btn-danger" type="button">X</button>
                      </td>
                    </tr>
                  </tbody>
                </table>

                <p class="text-end">
                  Total pagar: <span class="fw-bold">${{ total }}</span>
                </p>
                <button 
                  @click="$emit('empty-cart')"
                  class="btn btn-dark w-100 mt-3 p-2">
                  Empty Cart
                </button>
              </div>
            </div>
          </div>
        </nav>
      </div>
      <!--.row-->

      <div class="row mt-5">
        <div class="col-md-6 text-center text-md-start pt-5">
          <h1 class="display-2 fw-bold">{{ productShowCase.nombre }}</h1>
          <p class="mt-5 fs-5 text-white">
            {{ productShowCase.descripcion }}
          </p>
          <p class="text-primary fs-1 fw-black">$ {{ productShowCase.precio }}</p>
          <button
            @click="$emit('add-to-cart', productShowCase)"
            type="button"
            class="btn fs-4 bg-primary text-white py-2 px-5"
          >
            Add to cart
          </button>
        </div>
      </div>
    </div>

    <img
      class="header-guitarra"
      src="/img/header_guitarra.png"
      alt="imagen header"
    />
  </header>
</template>

<style lang="scss" scoped></style>

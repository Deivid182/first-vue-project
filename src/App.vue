<script setup>
import { ref, onMounted, watch } from 'vue';
import { db } from './data';
import ProductCard from './components/product-card.vue';
import Header from './components/header.vue';
import Footer from './components/footer.vue';

const products = ref([]);
const cart = ref([]);
const productShowCase = ref({});

watch(cart, () => {
  updateLS();
}, { deep: true });

onMounted(() => {
  productShowCase.value = db[3];
  
  products.value = db;
  const cartLS = localStorage.getItem('cart');

  if (cartLS) {
    cart.value = JSON.parse(cartLS);
  }
});


const updateLS = () => {
  localStorage.setItem('cart', JSON.stringify(cart.value));
};

const addToCart = (product) => {

  const index = cart.value.findIndex((p) => p.id === product.id);

  if (index === -1) {
    product.quantity = 1;
    cart.value.push(product);
  } else {
    cart.value[index].quantity++;
  }

};
const increment = (id) => {
  const product = cart.value.find((p) => p.id === id);

  product.quantity++;

};

const decrement = (id) => {

  const product = cart.value.find((p) => p.id === id);

  if (product.quantity > 1) {
    product.quantity--;
  } else {
    cart.value = cart.value.filter((p) => p.id !== id);
  }

};

const removeFromCart = (id) => {
  cart.value = cart.value.filter((p) => p.id !== id);

};

const emptyCart = () => {
  cart.value = [];
};

</script>

<template>
  <Header 
    :cart="cart" 
    :product-show-case="productShowCase"
    @empty-cart="emptyCart"
    @add-to-cart="addToCart"
    @increment="increment" 
    @decrement="decrement" 
    @remove-from-cart="removeFromCart"
  />
  <main class="container-xl mt-5">
    <h2 class="text-center">Nuestra Colección</h2>

    <div class="row mt-5">
      <ProductCard
        v-for="product in products"
        :key="product.id"
        :product="product"
        @add-to-cart="addToCart"
      />
    </div>
  </main>

  <Footer />
</template>

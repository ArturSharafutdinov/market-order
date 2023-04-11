<template>
  <div class="row justify-center">
    <q-card v-for="product in products" :key="product.id" class="q-ma-sm" style="max-width: 200px; max-height: 250px" @click="gotoProduct(product.id)">
      <img src="https://cdn.quasar.dev/img/mountains.jpg">

      <q-card-section>
        <div class="text-h6">{{ product.name }}</div>
        <div class="text-subtitle2">{{ product.description }}</div>
      </q-card-section>

      <q-card-section class="q-pt-none text-green-5">
        {{ product.price }}$
      </q-card-section>
    </q-card>
  </div>
</template>

<script lang="ts">

import { defineComponent } from 'vue';
import { useRouter } from 'vue-router';

export default defineComponent({
  props: {
    products: {
      type: [],
      required: false,
      default: []
    }
  },
  setup() {
    const router = useRouter();

    const gotoProduct = (productId) => {
      router.push(`/product/${productId}`);
    };

    return {
      gotoProduct
    }
  }
})

</script>

<template>
  <div style="display: flex; justify-content: center; align-items: center">
    <q-card style="min-width: 50%; max-width: 50%">
      <q-card-section v-if="order.size > 0">
        <product-list :products="order"/>
      </q-card-section>
      <q-card-section v-else>
        В данный момент у вас нет добавленных товаров в корзине!
      </q-card-section>
      <q-card-actions align="left">
        <q-btn disable>Price: {{price}}$</q-btn>
        <q-space/>
        <q-btn @click="makeOrder">Order</q-btn>
      </q-card-actions>
    </q-card>
  </div>
</template>

<script lang="ts">

import { computed, defineComponent, onBeforeUnmount, ref } from 'vue';
/* eslint-disable */
// @ts-ignore
import { orderService$ } from '@tko/market-store'
import ProductList from '@/views/ProductList.vue';
import axios from 'axios';

export default defineComponent({
  name: 'Order',
  components: { ProductList },
  setup() {
    const order = ref(new Set());
    const orderSubscription = orderService$.getOrder().subscribe(data => {
      if (data) {
        order.value = data;
      } else {
        order.value = new Set();
      }
    })

    const makeOrder = () => {
      try {
        const response = axios.post('http://localhost:8080/order', {
          products: Array.from(order.value)
        }).then((response) => {
          alert('Ваш заказ под номером: ' + response.data);
          orderService$.clearOrder();
        });
      } catch (error) {
        console.error(error);
      }
    }

    const price = computed<number>((): number => {
      return Array.from(order.value).reduce((a, b) => a + (b['price'] || 0), 0) as number;
    });

    onBeforeUnmount(() => {
      orderSubscription.unsubscribe();
    })

    return {
      order,
      price,
      makeOrder
    }
  }
})

</script>

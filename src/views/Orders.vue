<template>
  <div class="row justify-start">
    <q-card v-for="order in orders" :key="order.id" class="q-ma-md" style="max-width: 750px; max-height: 350px">
      <q-card-section>
        <div class="text-h6">Заказ №{{ order.id }}</div>
      </q-card-section>

      <q-card-section>
        <div class="row justify-center">
          <q-card v-for="product in order.products" :key="product.id" style="max-width: 100px; max-height: 100px" class="q-ma-sm">
            <q-img src="https://cdn.quasar.dev/img/mountains.jpg"/>
            <q-card-section style="font-size: 10px">
              {{product.name}}
            </q-card-section>
          </q-card>
        </div>
      </q-card-section>

      <q-card-actions class="text-green-5">
        <q-btn disable>{{ price(order) }}$</q-btn>
        <q-space/>
        <q-btn disable>{{ date(order) }}</q-btn>
      </q-card-actions>
    </q-card>
  </div>
</template>
<script>
import axios from 'axios';
import { ref } from 'vue';
import moment from 'moment';

export default {
  // eslint-disable-next-line vue/multi-word-component-names
  name: 'Orders',
  setup() {
    const orders = ref([]);
    axios.get('http://localhost:8080/orders').then(response => {
      orders.value = response.data;
    })

    const price = (order) => {
      return order.products.reduce((a,b) => a + b.price, 0);
    }

    const date = (order) => {
      return moment(String(order.orderDate)).format('MM/DD/YYYY hh:mm');
    }

    return {
      orders,
      price,
      date
    }
  }
};
</script>

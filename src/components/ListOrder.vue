<template>
  <div>
    <div class="alert alert-light text-center" role="alert" v-if="orderItems.length == 0">
      尚未建立訂單
    </div>
    <div class="card" v-else>
      <div class="card-body">
        <h3>訂單</h3>
        <table class="table">
          <thead>
            <tr>
              <th scope="col">品項</th>
              <th scope="col">數量</th>
              <th scope="col">小計</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(item, index) in orderItems" :key="index">
              <td>{{ item.name }}</td>
              <td>{{ item.quantities }}</td>
              <td>{{ item.quantities * item.price }}</td>
            </tr>
          </tbody>
        </table>
        <div class="text-end">備註: {{ props.orderNote }}</div>
        <div class="text-end">
          <h2>總計: ${{ totalPrice }}</h2>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { defineProps, computed } from 'vue'

const props = defineProps(['data', 'orderNote'])
const orderItems = props.data

const totalPrice = computed(() => {
  return orderItems.reduce((pre, next) => {
    return pre + next.price * next.quantities
  }, 0)
})
</script>

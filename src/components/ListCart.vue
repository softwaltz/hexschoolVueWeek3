<template>
  <div>
    <table class="table">
      <thead>
        <tr>
          <th scope="col" style="width: 60px">操作</th>
          <th scope="col" style="width: 150px">品項</th>
          <th scope="col">描述</th>
          <th scope="col" style="width: 100px">數量</th>
          <th scope="col" style="width: 60px">單價</th>
          <th scope="col" style="width: 60px">小計</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(item, index) in cartItems" :key="index">
          <td>
            <button type="button" class="btn btn-sm" @click="emits('removeDrink', index)">x</button>
          </td>
          <td>{{ item.name }}</td>
          <td>{{ item.description }}</td>
          <td>
            <select class="form-select" v-model="item.quantities">
              <option value="1">1</option>
              <option value="2">2</option>
              <option value="3">3</option>
              <option value="4">4</option>
              <option value="5">5</option>
              <option value="6">6</option>
              <option value="7">7</option>
              <option value="8">8</option>
              <option value="9">9</option>
              <option value="10">10</option>
            </select>
          </td>
          <td>{{ item.price }}</td>
          <td>{{ item.quantities * item.price }}</td>
        </tr>
      </tbody>
    </table>
    <div class="alert alert-info text-center" role="alert" v-if="cartItems.length == 0">
      請選擇商品
    </div>
    <div class="text-end" v-else>
      <h2>總計:{{ totalPrice }}</h2>
      <textarea class="form-control" placeholder="備註" v-model="note"></textarea>
      <br />
      <button class="btn btn-info" type="button" @click="emits('order', note)">送出</button>
    </div>
  </div>
</template>

<script setup>
import { ref, defineProps, defineEmits, computed } from 'vue'

const note = ref('')

const props = defineProps(['data', 'total'])
const cartItems = props.data

const emits = defineEmits(['removeDrink', 'order'])

const totalPrice = computed(() => {
  return cartItems.reduce((pre, next) => {
    return pre + next.price * next.quantities
  }, 0)
})
</script>

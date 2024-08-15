<script setup>
import { useItemsStore } from '@/stores/items'
import listDrinks from '@/components/ListDrinks.vue'

const itemsStore = useItemsStore()
const drinks = itemsStore.data
const cartItems = itemsStore.cartItems
const orderItems = itemsStore.orderItems

const addDrink = (index) => {
  cartItems.push({
    name: drinks[index].name,
    description: drinks[index].description,
    price: drinks[index].price,
    quantities: 1
  })
}
const removeDrink = (index) => {
  console.log(cartItems)
  cartItems.splice(index, 1)
}

const order = () => {
  orderItems.splice(0, orderItems.length)
  orderItems.push(...cartItems)
  itemsStore.orderNote = itemsStore.cartNote

  cartItems.splice(0, cartItems.length)
  itemsStore.cartNote = ''
}
</script>

<template>
  <div class="container">
    <div class="row">
      <div class="col-4">
        <list-drinks :data="drinks" @emit-add-drink-index="addDrink"></list-drinks>
      </div>
      <div class="col-8">
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
                <button type="button" class="btn btn-sm" @click="removeDrink(index)">x</button>
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
          <h2>總計:{{ itemsStore.cartTotalPrice }}</h2>
          <textarea
            class="form-control"
            placeholder="備註"
            v-model="itemsStore.cartNote"
          ></textarea>
          <br />
          <button class="btn btn-info" type="button" @click="order">送出</button>
        </div>
      </div>
    </div>
    <hr />
    <div class="row">
      <div class="col-2"></div>
      <div class="col-8">
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
            <div class="text-end">備註: {{ itemsStore.orderNote }}</div>
            <div class="text-end">
              <h2>總計: ${{ itemsStore.orderTotalPrice }}</h2>
            </div>
          </div>
        </div>
      </div>
      <div class="col-2"></div>
    </div>
  </div>
</template>

<style>
.item-style {
  padding: 15px;
}
</style>

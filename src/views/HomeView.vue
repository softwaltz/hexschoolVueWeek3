<script setup>
import { useItemsStore } from '@/stores/items'
import listDrinks from '@/components/ListDrinks.vue'
import listCart from '@/components/ListCart.vue'
import listOrder from '@/components/ListOrder.vue'

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
  cartItems.splice(index, 1)
}

const order = (data) => {
  orderItems.splice(0, orderItems.length)
  orderItems.push(...cartItems)
  itemsStore.cartNote = data

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
        <list-cart :data="cartItems" @remove-drink="removeDrink" @order="order"></list-cart>
      </div>
    </div>
    <hr />
    <div class="row">
      <div class="col-2"></div>
      <div class="col-8">
        <list-order :data="orderItems" :order-note="itemsStore.orderNote"></list-order>
      </div>
      <div class="col-2"></div>
    </div>
  </div>
</template>

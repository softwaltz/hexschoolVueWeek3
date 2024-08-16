<template>
  <div class="list-group">
    <div class="list-group-item item-style" v-for="(drink, index) in drinks" :key="index">
      <div class="row" @click="emits('emitAddDrinkIndex', index)">
        <div class="col-10">
          <h3>{{ drink.name }}</h3>
          {{ drink.description }}
        </div>
        <div class="col-2">${{ drink.price }}</div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { defineProps, defineEmits } from 'vue'

const props = defineProps(['data'])
const drinks = props.data

const emits = defineEmits(['emitAddDrinkIndex'])
</script>

<style>
.item-style {
  padding: 15px;
}
</style>

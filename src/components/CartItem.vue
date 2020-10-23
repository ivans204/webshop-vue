<template>
  <b-row class="cart-item">
    <b-col sm="1">
      <img class="btn-delete" @click="deleteItem(item)" :src="require('@/assets/x-circle.svg')" alt="">
    </b-col>

    <b-col>{{ item.name }}</b-col>
    <b-col>
      <button class="quantity-btn" @click="removeItemQuantity(item)">
        <img :src="require('@/assets/minus-circle.svg')" alt="">
      </button>
      <span>{{ item.quantity }}</span>
      <button class="quantity-btn" @click="addItemQuantity(item)">
        <img :src="require('@/assets/plus-circle.svg')" alt="">
      </button>
    </b-col>
    <b-col>{{ (item.price * item.quantity).toFixed(2) }} €</b-col>

  </b-row>
</template>

<script>
export default {
  name: 'Product',
  props: {
    item: Object,
    cart: Array
  },
  methods: {
    addItemQuantity(item) {
      item.quantity++
    },
    removeItemQuantity(item) {
      item.quantity > 1 ? item.quantity-- : this.$emit('getNewCart', item.id)
    },
    deleteItem(item) {
      this.$emit('getNewCart', item.id)
    }
  },
}
</script>

<style lang="scss" scoped>
.quantity-btn {
  border: none;
  background: transparent;

  &:focus {
    outline: none;
  }
}

.btn-delete {
  cursor: pointer;
}

.cart-item {
  padding-top: 20px;
  padding-bottom: 20px;
  border-bottom: 1px solid #dee2e6;
}
</style>

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
    <b-col>{{ item.price }} €</b-col>
  </b-row>
</template>

<script>
import productsData from "@/productsData";

export default {
  name: 'Product',
  props: {
    item: Object,
    cart: Array
  },
  methods: {
    addItemQuantity(item) {
      item.quantity++
      this.updateItemPrice(item)
    },
    removeItemQuantity(item) {
      item.quantity > 1 ? item.quantity-- : this.$emit('getNewCart', item.id)
      this.updateItemPrice(item)
    },
    deleteItem(item) {
      this.$emit('getNewCart', item.id)
    },
    updateItemPrice(item) {
      const realPrice = productsData.find(product => product.id === item.id)

      if (item.quantity % item.promotionQuantity === 0) {
        item.price = item.promotionPrice * (item.quantity / item.promotionQuantity)
      } else {
        item.price = (realPrice.price * item.quantity).toFixed(2)
      }
    }
  },
  mounted() {
    this.updateItemPrice(this.item);
  }
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

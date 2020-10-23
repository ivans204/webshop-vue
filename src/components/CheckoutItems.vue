<template>
  <b-col offset="2" sm="5">

    <b-row class="checkout-table font-weight-bold">
      <b-col>Name</b-col>
      <b-col class="text-center">Quantity</b-col>
      <b-col>Price</b-col>
    </b-row>
    <b-row v-for="(item, index) in cart" :key="index" class="checkout-table">
      <b-col>{{ item.name }}</b-col>
      <b-col class="text-center">{{ item.quantity }}</b-col>
      <b-col>{{ (item.price * item.quantity).toFixed(2) }} €</b-col>
    </b-row>
    <b-row class="checkout-table">
      <b-col>Promotions:</b-col>
      <b-col>
        <ul class="promotions">
          <li class="promotions-item" v-for="(code, index) in activeCodes" :key="index">{{ code.code }}</li>
        </ul>
      </b-col>
      <b-col class="text-right">Total:</b-col>
      <b-col class="font-weight-bold">{{ cartPriceTotal }} €</b-col>
    </b-row>

  </b-col>
</template>

<script>
export default {
  name: "CheckoutForm",
  props: {},
  data: () => ({
    cart: JSON.parse(localStorage.getItem('cart')) || [],
    activeCodes: JSON.parse(localStorage.getItem('activeCodes')) || [],
  }),
  methods: {},
  computed: {
    cartPriceTotal() {
      let cartTotal = this.cart.reduce((total, product) => (product.price * product.quantity) + total, 0)

      if (this.activeCodes.length) {
        this.activeCodes.forEach(item => {
          if (item.discount < 1 && !item.repeatable) {
            cartTotal = cartTotal * item.discount
          } else if (item.discount < 1 && item.repeatable) {
            cartTotal = cartTotal * item.discount
          } else if (item.discount > 1 && item.repeatable) {
            cartTotal = cartTotal - item.discount
          }
        })
      }
      return cartTotal.toFixed(2)
    },
  }
}
</script>

<style scoped lang="scss">
.checkout-table {
  padding: 15px 0;
  border-bottom: 1px solid #dee2e6;
}

.promotions {
  .promotions-item {
    list-style: none;
  }
}
</style>
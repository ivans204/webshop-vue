<template>
  <div id="app">
    <div id="nav">
      <b-container>
        <b-row class="justify-content-between">
          <router-link to="/">Shop</router-link>
          <router-link to="/cart">

            <div class="shoppingCart">
              <span class="mr-1">Cart</span>
              <img :src="require('@/assets/cart.svg')" alt="">
              <span class="shoppingCart-quantity">{{ cartQuantity }}</span>
            </div>

          </router-link>
        </b-row>
      </b-container>
    </div>
    <router-view @getAmount="updateCart($event)"/>
  </div>
</template>

<script>
import productsData from "@/productsData";

export default {
  data: () => ({
    items: productsData,
    cart: JSON.parse(localStorage.getItem('cart')) || [],
  }),
  methods: {
    setData(data = this.cart) {
      localStorage.setItem('cart', JSON.stringify(data))
    },
    updateCart(payload) {
      this.setData(payload)
      this.cart = JSON.parse(localStorage.getItem('cart'))
    }
  },
  computed: {
    cartQuantity() {
      return this.cart.reduce((total, product) => product.quantity + total, 0)
    },
  },
}
</script>

<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@400;700&display=swap');

#app {
  font-family: 'Poppins', sans-serif;
}

#nav {
  box-shadow: 0 0 10px rgba(0, 0, 0, .02), 0 10px 20px rgba(0, 0, 0, .02);
  padding: 20px;
}

.shoppingCart {
  position: relative;
}

.shoppingCart-quantity {
  height: 30px;
  width: 30px;
  position: absolute;
  right: 0;
  top: 30%;
  text-align: center;
}
</style>

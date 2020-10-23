<template>
  <div class="cart">
    <b-container>

      <b-row class="align-items-center border-bottom cart-item-title">
        <b-col offset="1">Name</b-col>
        <b-col>Quantity</b-col>
        <b-col>Price</b-col>
      </b-row>

      <h3 class="text-center mt-5 font-weight-bold" v-if="!cart.length">
        Cart is empty! :(
      </h3>

      <div @click="updateSum">
        <CartItem
            v-for="(item,index) in cart"
            :key="index"
            :cart="cart"
            :item="item"
            @getNewCart="updateCart($event)"
        ></CartItem>
      </div>

      <Promotions v-if="cart.length" :cart="cart"></Promotions>

      <b-row>
        <b-col class="d-flex total">
          <router-link class="btn-checkout" to="/checkout" v-if="cart.length">
            Checkout
          </router-link>
        </b-col>
      </b-row>

    </b-container>
  </div>
</template>

<script>
import CartItem from "@/components/CartItem";
import Promotions from "@/components/Promotions";

export default {
  name: 'Cart',
  components: {
    CartItem,
    Promotions
  },
  data: () => ({
    cart: JSON.parse(localStorage.getItem('cart')) || [],
  }),
  methods: {
    setData(data = this.cart) {
      localStorage.setItem('cart', JSON.stringify(data))
    },
    updateCart(payload) {
      this.setData(this.cart.filter(item => item.id !== payload))
      this.cart = JSON.parse(localStorage.getItem('cart'))
    },
    updateSum() {
      this.$emit('getAmount', this.cart)
    },
  },
}
</script>

<style scoped lang="scss">
.cart-item-title {
  padding-top: 20px;
  padding-bottom: 20px;
  font-weight: bold;
}

.cart-item {
  padding-top: 20px;
  padding-bottom: 20px;
}

.total {
  padding: 20px 0;
  flex-direction: column;
  align-items: flex-end;

  .btn-checkout {
    border: none;
    background: #4fc3f7;
    color: white;
    padding: 15px 30px;
    border-radius: 5px;
    font-weight: bold;

    &:focus {
      outline: none;
    }
  }
}
</style>

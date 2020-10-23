<template>
  <div class="home">
    <b-container>
      <b-row @click="updateSum">

        <ShopProduct v-for="(product, index) in products" :key="index" :product="product" :cart="cart"></ShopProduct>

      </b-row>
    </b-container>
  </div>
</template>

<script>
import productsData from '@/productsData'
import ShopProduct from "@/components/ShopProduct";

export default {
  name: 'Home',
  components: {
    ShopProduct
  },
  data: () => ({
    products: productsData,
    cart: JSON.parse(localStorage.getItem('cart')) || []
  }),
  methods: {
    updateSum() {
      this.$emit('getAmount', this.cart)
    }
  },
}
</script>

<style scoped lang="scss">
.product-add {
  border: none;
  padding: 10px 15px;
  background-color: #ff5722;
  color: white;
  border-radius: 10px;

  &:focus {
    outline: none;
  }
}
</style>

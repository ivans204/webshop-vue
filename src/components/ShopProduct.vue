<template>
  <div class="product">
    <img class="product-image" :src="require('@/assets/placeholder.png')" alt="">
    <div class="product-details">
      <h3>{{ product.name }}</h3>

      <div class="product-details_cart">
        <p class="product-price m-0">€{{ product.price }}</p>
        <button class="product-add" @click="addItem(product)">Add to cart</button>
      </div>
    </div>
  </div>
</template>

<script>
import productsData from "@/productsData";

export default {
  name: 'Product',
  props: {
    product: Object,
    cart: Array
  },
  data: () => ({}),
  methods: {
    setData(data = this.cart) {
      localStorage.setItem('cart', JSON.stringify(data))
    },
    addItem(item) {
      const cartItem = this.cart.find(cartItem => cartItem.id === item.id)

      if (!cartItem) {
        item = {...item, quantity: 1}
        this.cart.push(item)
      } else {
        item = {...item, quantity: cartItem.quantity++}
        cartItem.price = item.price * cartItem.quantity
        this.updateItemPrice(cartItem)
      }
      this.setData()
    },
    updateItemPrice(item) {
      const realPrice = productsData.find(product => product.id === item.id)

      if (item.quantity % item.promotionQuantity === 0) {
        item.price = item.promotionPrice * (item.quantity / item.promotionQuantity)
      } else {
        item.price = (realPrice.price * item.quantity).toFixed(2)
      }
    }
  }
}
</script>

<style lang="scss" scoped>
.product {
  margin: 50px;
  box-shadow: 0 0 10px rgba(0, 0, 0, .02), 0 10px 20px rgba(0, 0, 0, .02);

  .product-image {
    width: 100%;
  }

  .product-details {
    padding: 0 25px;

    h3 {
      text-align: center;
      font-size: 1.5rem;
      font-weight: bold;
      margin: 20px 0;
    }
  }

  .product-details_cart {
    display: flex;
    justify-content: space-evenly;
    margin-bottom: 20px;
    align-items: center;

    .product-price {
    }

    .product-add {
      border: none;
      padding: 10px 15px;
      background-color: #4fc3f7;
      color: white;
      border-radius: 10px;

      &:focus {
        outline: none;
      }
    }
  }
}
</style>

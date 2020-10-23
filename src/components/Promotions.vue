<template>
  <b-row class="cart-item">

    <b-col class="text-right">Discount code:</b-col>

    <b-col>
      <input class="input-code" type="text" v-model="voucher" :disabled="inputDisabled">
      <button class="btn-add" @click="addPromotions" :disabled="inputDisabled">Add code</button>
    </b-col>
    <b-col>
      <ul class="code-list">
        <li class="code-list-item" v-for="(code, index) in activeCodes" :key="index">
          {{ code.code }}
          <button class="code-list_btn-remove" @click="removePromotion(code)">
            <img :src="require('@/assets/x-circle.svg')" alt="">
          </button>
        </li>
      </ul>
    </b-col>

    <b-col class="text-right">Total:</b-col>

    <b-col class="font-weight-bold">
      {{ cartPriceTotal }} €
    </b-col>
  </b-row>
</template>

<script>
import discountCodes from "@/discountCodes";

export default {
  name: 'Product',
  data: () => ({
    codes: discountCodes,
    voucher: '',
    inputDisabled: false,
    activeCodes: JSON.parse(localStorage.getItem('activeCodes')) || [],
  }),
  props: {
    cart: Array
  },
  methods: {
    setData(data = this.activeCodes) {
      localStorage.setItem('activeCodes', JSON.stringify(data))
    },
    getData() {
      this.activeCodes = JSON.parse(localStorage.getItem('activeCodes'))
    },
    addPromotions() {
      this.codes.map(discount => {
        if (discount.code === this.voucher && discount.repeatable === false) {
          this.activeCodes = []
          this.activeCodes.push(discount)
          this.inputDisabled = true
        } else if (discount.code === this.voucher && !this.activeCodes.some(item => item.code === discount.code )) {
          this.activeCodes.push(discount)
        }
      })
      this.voucher = ''
      this.setData()
      this.getData()
    },
    removePromotion(promotion) {
      if (promotion.repeatable === false) this.inputDisabled = false
      this.setData(this.activeCodes.filter(item => item !== promotion))
      this.getData()
    }
  },
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

<style lang="scss" scoped>

.input-code {
  padding: 5px 10px;
  text-transform: uppercase;

  &:focus {
    outline: none;
  }
}

.btn-add {
  margin-top: 20px;
  border: 1px solid #4fc3f7;
  background: transparent;
  border-radius: 5px;
  width: 100%;
  padding: 5px 10px;

  &:focus {
    outline: none;
  }
}

.code-list {
  .code-list-item {
    display: flex;
    justify-content: space-between;
  }

  .code-list_btn-remove {
    border: none;
    background: transparent;

    &:focus {
      outline: none;
    }
  }
}
</style>

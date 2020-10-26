<template>
  <b-row class="cart-item">

    <b-col class="text-right">Discount code:</b-col>

    <b-col>
      <input :disabled="inputDisabled" class="input-code" type="text" v-model="voucher">
      <button :disabled="inputDisabled" @click="addPromotions" class="btn-add">Add code</button>
    </b-col>
    <b-col>
      <ul class="code-list">
        <li :key="index" class="code-list-item" v-for="(code, index) in activeCodes">
          {{ code.code }}
          <button @click="removePromotion(code)" class="code-list_btn-remove">
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
        } else if (discount.code === this.voucher && !this.activeCodes.some(item => item.code === discount.code)) {
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
      let cartTotal = this.cart.reduce((total, product) => parseFloat(product.price) + total, 0)

      if (this.activeCodes.length) {
        const codesDeduct = this.activeCodes.filter(item => item.discount > 1)
        const codesPercentage = this.activeCodes.filter(item => item.discount < 1)

        codesDeduct.forEach(item => cartTotal -= item.discount)
        codesPercentage.forEach(item => cartTotal *= item.discount)
      }
      return cartTotal.toFixed(2)
    },
  },
  mounted() {
    if (this.activeCodes.some(item => item.repeatable === false)) {
      this.inputDisabled = true
    }
  }
}
</script>

<style lang="scss" scoped>

.input-code {
  padding: 5px 10px;

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

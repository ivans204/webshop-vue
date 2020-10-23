<template>
  <div class="home">
    <b-container>
      <b-row style="margin-top: 15px">
        <CheckoutForm></CheckoutForm>
        <CheckoutItems></CheckoutItems>
      </b-row>
    </b-container>
  </div>
</template>

<script>

import CheckoutForm from "@/components/CheckoutForm";
import CheckoutItems from "@/components/CheckoutItems";

export default {
  name: 'Home',
  components: {
    CheckoutForm,
    CheckoutItems
  },
}
</script>

<style scoped lang="scss">
</style>

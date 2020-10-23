<template>
  <b-col sm="5">
    <form class="checkout-form">

      <label for="customer-email">Email</label>
      <input class="checkout-form_input" id="customer-email" type="email" required>

      <label for="customer-address">Address</label>
      <input class="checkout-form_input" id="customer-address" type="text" required>

      <label for="customer-card">Credit Card</label>
      <input class="checkout-form_input" id="customer-card" type="number">

      <button class="btn-submit" type="submit">Submit</button>

    </form>
  </b-col>

</template>

<script>
export default {
  name: "CheckoutForm"
}
</script>

<style scoped lang="scss">
.checkout-form {
  display: flex;
  flex-direction: column;
  margin: 15px;

  .checkout-form_input {
    border: 1px solid #dee2e6;
    border-radius: 5px;
    margin-bottom: 15px;
    padding: 10px;
  }
}

.btn-submit {
  margin: 15px 0;
  width: 150px;
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
</style>
<template>
  <div class="hello">
    <input v-model="message" placeholder="edit me" />
    <p>Message is: {{ message }}</p>
    <CountrySelector></CountrySelector>
    <PaymentMethods></PaymentMethods>
    <PaymentMethodForm></PaymentMethodForm>
  </div>
</template>

<script>
import PaymentMethods from "./PaymentMethods";
import CountrySelector from "./CountrySelector";
import PaymentMethodForm from "./PaymentMethodForm";
export default {
  name: "PaymentWidget",
  data() {
    return { message: "" };
  },
  components: {
    PaymentMethods,
    CountrySelector,
    PaymentMethodForm,
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
p {
  color: aquamarine;
}
</style>

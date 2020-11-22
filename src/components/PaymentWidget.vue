<template>
  <div class="hello">
    <h1>Payment Widget</h1>
    <h3>Amount: {{amount}}</h3>
    <CountrySelector v-on:countryToHome="countryChange" />
    <PaymentMethods
      v-bind:methodsData="paymentMethods"
      v-on:methodsToHome="methodsChange"
    />
    <CreditCardForm v-if="selectedMethod" :amountInput="amount" />
  </div>
</template>

<script>
import PaymentMethods from "./PaymentMethods";
import CountrySelector from "./CountrySelector";
import CreditCardForm from "./CreditCardForm";
import axios from "axios";
export default {
  name: "PaymentWidget",
  data() {
    return {
      selectedCode: "VN",
      paymentMethods: [],
      selectedMethod: "",
      amount: "5 USD",
    };
  },
  components: {
    PaymentMethods,
    CountrySelector,
    CreditCardForm,
  },
  methods: {
    methodsChange(idMethod) {
      this.selectedMethod = idMethod;
    },
    async countryChange(value) {
      this.selectedMethod = '';
      this.selectedCode = value;
      const params = {
        params: {
          country_code: value,
          key: "a665e3b3b823d3298633a6af0e9c46a3",
          sign_version: 2,
        },
      };
      const url = `https://api.paymentwall.com/api/payment-systems/`;
      await axios
        .get(url, params)
        .then((response) => {
          if (response.data) {
            this.paymentMethods = response.data;
          }
        })
        .catch((err) => console.log(err));
    },
  },
  created() {
    this.countryChange(this.selectedCode);
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
p {
  color: aquamarine;
}
</style>

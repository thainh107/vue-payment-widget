<template>
  <div class="hello">
    <h1>Payment Widget</h1>
    
    <CountrySelector v-on:countryToHome="countryChange" />
    <PaymentMethods
      v-bind:methodsData="paymentMethods"
      v-on:methodsToHome="methodsChange"
    />
    <PaymentMethodForm />
  </div>
</template>

<script>
import PaymentMethods from "./PaymentMethods";
import CountrySelector from "./CountrySelector";
import PaymentMethodForm from "./PaymentMethodForm";
import axios from "axios";
export default {
  name: "PaymentWidget",
  data() {
    return {
      userData: {
        email: "",
        password: "",
        satisfaction: "",
      },
      selectedCode: "VN",
      paymentMethods: [],
      selectedMethod: "",
    };
  },
  components: {
    PaymentMethods,
    CountrySelector,
    PaymentMethodForm,
  },
  methods: {
    methodsChange(idMethod) {
      this.selectedMethod = idMethod;
    },
    async countryChange(value) {
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

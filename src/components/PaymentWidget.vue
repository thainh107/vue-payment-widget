<template>
  <div class="hello">
    <input v-model="message" placeholder="edit me" />
    <CountrySelector v-on:childToParent="onChildClick"></CountrySelector>
    <PaymentMethods v-model="PaymentMethods"></PaymentMethods>
    <PaymentMethodForm v-model="age"></PaymentMethodForm>
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
      message: "",
      checkboxOptions: [],
      radioBoxOption: "Male",
      age: "",
      selectedCode: "",
      PaymentMethods: [],
    };
  },
  components: {
    PaymentMethods,
    CountrySelector,
    PaymentMethodForm,
  },
  methods: {
    async onChildClick(value) {
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
            this.PaymentMethods = response.data;
            console.log(this.PaymentMethods);
          }
        })
        .catch((err) => console.log(err));
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
p {
  color: aquamarine;
}
</style>

<template>
<div>
  <p v-if="errors.length">
    <b>Please correct the following error(s):</b>
    <ul>
      <li v-for="error in errors" v-bind:key="error">{{ error }}</li>
    </ul>
  </p>

  <p>
    <label for="name">Card holder name</label>
    <input
      id="name"
      v-model="name"
      type="text"
      name="name"
      v-on:keypress="isLetter($event)"
    >
  </p>

  <p>
    <label for="cardnumber">Card Number</label>
    <input
      id="cardnumber"
      v-model="cardnumber"
      type="number"
      name="cardnumber"
      v-on:keypress="isNumber($event)"
    >
  </p>

  <p>
    <label for="expMonth">Exp Date</label>
    <input
      id="expMonth"
      v-model="expMonth"
      type="number"
      name="expMonth"
      class="short-number"
      placeholder="MM"
      v-on:keypress="isNumber($event)"
    > /
    <input
      id="expYear"
      v-model="expYear"
      type="expYear"
      name="expYear"
      class="short-number"
      placeholder="YYYY"
      v-on:keypress="isNumber($event)"
    >
  </p>

  <p>
    <label for="cvv">CVV</label>
    <input
      id="cvv"
      v-model="cvv"
      type="number"
      name="cvv"
      class="short-number"
      v-on:keypress="isNumber($event)"
    >
  </p>

  <button v-on:click="checkForm">Payment for 5 USD</button>
</div>
</template>

<script>
export default {
  name: "PaymentMethodForm",
  data() {
    return {
      errors: [],
      name: null,
      cardnumber: null,
      expMonth: null,
      expYear: null,
      cvv: null,
    };
  },
  methods: {
    checkForm() {
      // this.validateExpDate();
      if (!this.validateCardNumber(this.cardnumber)) {
        alert("Wrong card number");
        return;
      } else {
        console.log("card number is valid")
      }

      // e.preventDefault();
    },
    validateExpDate() {
      var today, someday;
      today = new Date();
      someday = new Date();
      someday.setFullYear(this.expYear, this.expMonth, 1);
      console.log(today);
      console.log(someday);
      if (someday < today) {
        alert(
          "The expiry date is before today's date. Please select a valid expiry date"
        );
        return;
      }
    },
    isLetter(e) {
      let char = String.fromCharCode(e.keyCode); // Get the character
      if (/^[A-Za-z]+$/.test(char)) return true;
      // Match with regex
      else e.preventDefault(); // If not match, don't add to input text
    },
    isNumber(evt) {
      evt = evt ? evt : window.event;
      var charCode = evt.which ? evt.which : evt.keyCode;
      if (charCode > 31 && (charCode < 48 || charCode > 57)) {
        evt.preventDefault();
      } else {
        return true;
      }
    },
    luhnCheck(val) {
      var sum = 0;
      for (var i = 0; i < val.length; i++) {
        var intVal = parseInt(val.substr(i, 1));
        if (i % 2 == 0) {
          intVal *= 2;
          if (intVal > 9) {
            intVal = 1 + (intVal % 10);
          }
        }
        sum += intVal;
      }
      return sum % 10 == 0;
    },
    validateCardNumber(number) {
      var regex = new RegExp("^[0-9]{16}$");
      if (!regex.test(number)) return false;

      return this.luhnCheck(number);
    },
  },
};
</script>

<style scoped>
.short-number {
  width: 35px;
}
</style>
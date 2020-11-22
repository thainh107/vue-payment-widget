<template>
  <div class="card-form">
    <div class="card-form-inner">
      <div class="card-input">
        <label for="cardName" class="card-input-label"> Card Owner </label>
        <input autofocus id="cardName" class="card-input-input" v-model="name" autocomplete="off" v-on:keypress="isLetter($event)" placeholder="Ex: John Smith" />
      </div>
      <div class="card-input">
        <label for="cardNumber" class="card-input-label"> Card Number </label>
        <input v-model="cardNumber" id="cardNumber" class="card-input-input" autocomplete="off" v-on:keypress="isNumber($event)" placeholder="XXXX XXXX XXXX XXXX" @focusout="validateCardNumber($event)" />
      </div>
      <div v-if="errorCardNumber !== ''" class="error-warning">
        <label>* {{ errorCardNumber }}</label>
      </div>
      <div class="card-input">
        <label for="cardMonth" class="card-input-label"> Expiration Date </label>
        <div class="card-input-exp">
          <select class="card-exp" id="cardMonth" v-model="expireMonth" v-on:change="validateExpDate">
            <option value="" disabled selected>Month</option>
            <option v-for="n in 12" :value="n < 10 ? '0' + n : n" :key="n">
              {{ 10 > n ? "0" + n : n }}
            </option>
          </select>
          <select class="card-exp year" id="cardYear" v-model="expireYear" v-on:change="validateExpDate">
            <option value="" disabled selected>Year</option>
            <option v-for="(n, $index) in 12" :value="$index + currentYear" :key="n">
              {{ $index + currentYear }}
            </option>
          </select>
        </div>
      </div>

      <div v-if="errorExpireDate !== ''" class="error-warning">
        <label>* {{ errorExpireDate }}</label>
      </div>
      <div class="card-input">
        <label for="cardCvv" class="card-input-label">CVV</label>
        <div class="card-input-exp">
          <input class="card-cvv" id="cardCvv" v-model="cvv" autocomplete="off" v-on:keypress="isNumber($event)" @focusout="validateCVV($event)" />
        </div>
      </div>
      <div v-if="errorCVV !== ''" class="error-warning">
        <label>* {{ errorCVV }}</label>
      </div>
      <button @click="submitCard" class="card-form__button">Pay {{ amountInput }}</button>
    </div>
    <modal v-show="isModalVisible" @close="closeModal" />
  </div>
</template>

<script>
import modal from "./modal.vue";
export default {
  components: {
    modal,
  },
  props: {
    amountInput: String,
  },
  data() {
    return {
      cardNumber: "",
      expireMonth: "",
      expireYear: "",
      name: "",
      cvv: "",
      currentYear: new Date().getFullYear(),
      errorCardNumber: "",
      errorExpireDate: "",
      errorCVV: "",
      isModalVisible: false,
    };
  },
  methods: {
    validateInput() {
      this.validateCardNumber();
      this.validateExpDate();
      this.validateCVV();
      if (!this.errorCVV && !this.errorCardNumber && !this.errorExpireDate) {
        this.showModal();
      }
    },
    validateCVV() {
      this.cvv == "" ? (this.errorCVV = "Wrong CVV") : (this.errorCVV = "");
    },
    submitCard() {
      this.validateInput();
    },
    validateExpDate() {
      var today, someday;
      today = new Date();
      someday = new Date();
      someday.setFullYear(this.expireYear, this.expireMonth, 1);
      if (someday < today) {
        this.errorExpireDate = "Wrong expired date";
        return;
      } else {
        console.log("exp date is valid");
        this.errorExpireDate = "";
      }
    },
    isLetter(e) {
      let char = String.fromCharCode(e.keyCode); // Get the character
      if (/^[A-Za-z, ]+$/.test(char)) return true;
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
    luhnCheck(num) {
      let arr = (num + "")
        .split("")
        .reverse()
        .map((x) => parseInt(x));
      let lastDigit = arr.splice(0, 1)[0];
      let sum = arr.reduce((acc, val, i) => (i % 2 !== 0 ? acc + val : acc + ((val * 2) % 9) || 9), 0);
      sum += lastDigit;
      return sum % 10 === 0;
    },
    validateCardNumber() {
      const number = this.cardNumber;
      const regex = new RegExp("^[0-9]{15,16}$");
      console.log(this.luhnCheck(number));
      if (!regex.test(number)) {
        return (this.errorCardNumber = "Wrong Card number");
      } else {
        this.luhnCheck(number) ? (this.errorCardNumber = "") : (this.errorCardNumber = "Wrong Card number");
      }
    },
    showModal() {
      this.isModalVisible = true;
    },
    closeModal() {
      this.isModalVisible = false;
    },
  },
};
</script>

<style scoped>
.card-form {
  max-width: 570px;
  margin: auto;
  width: 100%;
}

.card-input {
  display: flex;
  flex-direction: row;
  padding-bottom: 10px;
  align-items: center;
}
.card-input-label {
  flex: 2;
  font-size: 18px;
  color: #1a3b5d;
}
.card-input-input {
  flex: 5;
  height: 30px;
  border-radius: 5px;
  box-shadow: none;
  border: 1px solid #ced6e0;
  font-size: 18px;
  padding: 5px 15px;
  background: none;
  color: #1a3b5d;
}

.card-input-exp {
  flex: 5;
  margin-left: -11px;
  font-size: 18px;
  padding: 5px 0;
  display: flex;
  height: 30px;
}
.card-exp {
  min-width: 100px;
  height: 30px;
  border-radius: 5px;
  box-shadow: none;
  border: 1px solid #ced6e0;
  font-size: 18px;
  background: none;
  color: #1a3b5d;
  padding: 0 5px;
}
.year {
  margin-left: 10px;
}
.card-cvv {
  max-width: 100px;
  height: 30px;
  border-radius: 5px;
  box-shadow: none;
  border: 1px solid #ced6e0;
  font-size: 18px;
  color: #1a3b5d;
  padding: 0 5px;
}

.error-warning {
  font-size: 18px;
  color: red;
  font-weight: bold;
}

.card-form__button {
  width: 100%;
  height: 55px;
  background: #38a294;
  border: none;
  border-radius: 5px;
  font-size: 22px;
  font-weight: 500;
  box-shadow: 3px 10px 20px 0px rgba(35, 100, 210, 0.3);
  color: #fff;
  margin: 10px 0;
  cursor: pointer;
}
</style>

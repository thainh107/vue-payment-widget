<template>
  <div>
    <label class="title">Country Selector</label>
    <select v-model="selectedCode" v-on:change="emitToParent" 
        class="country-option">
      <option
        v-for="item in priorities"
        v-bind:key="item.code"
        v-bind:value="item.code"
      >
        {{ item.name }}
      </option>
    </select>
  </div>
</template>

<script>
export default {
  name: "CountrySelector",
  data() {
    return {
      priorities: [
        {
          name: "Viet Nam",
          code: "VN",
          states: null,
        },
        {
          name: "United States",
          code: "US",
          states: null,
        },
        {
          name: "Turks and Caicos Islands",
          code: "TC",
          states: null,
        },
        {
          name: "Chad",
          code: "TD",
          states: null,
        },
        {
          name: "Thailand",
          code: "TH",
          states: null,
        },
      ],
      selectedCode: "VN",
    };
  },
  methods: {
    emitToParent() {
      this.$emit("countryToHome", this.selectedCode);
    },
  },
};
</script>

<style scoped>
.title {
  margin-right: 10px;
  font-size: 18px;
  color: #1a3b5d;
}
.country-option {
  font-size: 18px;
  color: #1a3b5d;
  border: 1px solid #ced6e0;
  border-radius: 3px;
}
</style>
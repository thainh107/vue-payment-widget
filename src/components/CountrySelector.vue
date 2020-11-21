<template>
  <div>
    <label class="title">Country Selector</label>
    <select v-model="selectedCode" v-on:change="emitToParent">
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
}

</style>
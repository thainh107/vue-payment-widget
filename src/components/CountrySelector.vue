<template>
  <div>
    <select v-model="selected">
      <option disabled value="">Please select Country</option>
      <option>A</option>
      <option>B</option>
      <option>C</option>
    </select>
    <span>Selected: {{ selected }}</span>
  </div>
</template>

<script>
export default {
  name: "CountrySelector",
  data() {
    return {
      selected: "",
    };
  },
};
</script>
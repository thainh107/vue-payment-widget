<template>
  <div>
    <span>Selected: {{ selected }}</span>
    <div
      v-for="method in methodsData"
      v-bind:key="method.id"
      v-bind:value="method.id"
      v-on:click="selectMethod(method.id)"
    >
      <img :src="method.img_url" :alt="method.img_class" width="100" />
    </div>
  </div>
</template>

<script>
export default {
  name: "PaymentMethods",
  data() {
    return {
      selected: "",
    };
  },
  props: ["methodsData"],
  methods: {
    selectMethod(id) {
      this.selected = id;
      this.$emit("methodsToHome", this.selected);
    }
  }
};
</script>
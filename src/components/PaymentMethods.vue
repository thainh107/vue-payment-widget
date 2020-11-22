<template>
  <div class="wrapper">
    <p>
      <label for="labelMethods" class="lbTitle"
        >Choose your payment method</label
      >
    </p>
    <div class="container">
      <div
        v-for="method in methodsData"
        v-bind:key="method.id"
        v-bind:value="method.id"
        v-on:click="selectMethod(method.id)"
        class="wrapper-img"
      >
        <img
          :src="method.img_url"
          :alt="method.img_class"
          v-bind:style="
            selected === method.id
              ? 'filter: grayscale(0);'
              : 'filter: grayscale(100%)'
          "
        />
      </div>
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
    },
  },
  watch: {
    methodsData(newVal, oldVal) {
      console.log("Prop changed: ", newVal, " | was: ", oldVal);
      this.selected = "";
    },
  },
};
</script>

<style scoped>
.wrapper {
  padding: 15px 0;
}
.container {
  display: flex;
  flex-direction: row;
  justify-content: space-evenly;
}
.wrapper-img {
  padding: 5px;
  border-radius: 10px;
  border: 1px solid #ccc;
}
.lbTitle {
  font-size: 18px;
  color: #1a3b5d;
}
</style>
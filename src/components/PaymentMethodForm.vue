<template>
  <div>
    <input v-model.number="age" type="number" />
  </div>
</template>

<script>
export default {
  name: "PaymentMethodForm",
  props: {
    age: Number,
  },
};
</script>
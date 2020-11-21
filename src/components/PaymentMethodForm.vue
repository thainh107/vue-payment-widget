<template>
  <div>
    <input v-bind:age="age" type="number" v-on:change="$emit('change', $event.target.value)" />
  </div>
</template>

<script>
export default {
  name: "PaymentMethodForm",
  props: {
    age: String
  },
  model: {
    prop: 'age',
    event: 'change'
  },
};
</script>
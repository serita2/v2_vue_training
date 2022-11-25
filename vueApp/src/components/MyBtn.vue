<template>
  <button @click="click" :disabled="activateSubmit"><slot /></button>
</template>

<script>
export default {
  name: "MyBtn",
  props: {
    activateSubmit: { type: Boolean }
  },
  methods: {
    click() {
      this.$emit("click");
    }
  }
};
</script>

<style scoped></style>

<template>
  <label><slot></slot></label>
</template>

<script>
export default {
  name: "MyLabel"
};
</script>

<style scoped lang="scss">
label {
  display: block;
  font-weight: bold;
  padding-right: 10px;
}
</style>

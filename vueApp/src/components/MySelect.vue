<template>
  <fieldset :style="str">
    <select :name="name" @change="updateValue">
      <template v-for="(option, index) in roomType">
        <option :value="option.value" @change="updateValue" :key="index">
          {{ option.label }}
        </option>
      </template>
    </select>
  </fieldset>
</template>

<script>
export default {
  name: "MySelect",
  props: {
    value: { type: String, require: true },
    roomType: { type: Array, require: true },
    name: { type: String, require: true },
    str: { type: String, require: true }
  },
  methods: {
    updateValue: function(e) {
      this.$emit("input", e.target.value);
    }
  },
  mounted() {
    this.$emit("input", this.roomType[0].value);
  }
};
</script>

<style scoped></style>

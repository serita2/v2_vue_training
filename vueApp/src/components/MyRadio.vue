<template>
  <fieldset :style="str">
    <div v-for="(option, index) in sex" :key="index">
        <input
          type="radio"
          :name="name"
          :value="option.value"
          @change="updateValue"
        />{{ option.label }}
    </div>
  </fieldset>
</template>

<script>
export default {
  name: "MyRadio",
  props: {
    value: { type: String, require: true },
    sex: { type: Array, require: true },
    name: { type: String, require: true },
    str: { type: String, require: true }
  },
  methods: {
    updateValue: function(e) {
      this.$emit("input", e.target.value);
    }
  }
};
</script>

<style scoped lang="scss">

</style>

<template>
  <fieldset :style="str">
    <div v-for="(option, index) in meal" :key="index">
        <input
          type="checkbox"
          :name="name"
          :value="option.value"
          @change="updateValue"
        />{{ option.label }}
    </div>
  </fieldset>
</template>

<script>
export default {
  name: "MyCheckbox",
  props: {
    meal: { type: Array, require: true },
    name: { type: String, require: true },
    str: { type: String, require: true }
  },
  data() {
    return {
      values: []
    };
  },
  methods: {
    updateValue: function(e) {
      if (e.target.checked) {
        this.values.push(e.target.value);
      } else {
        this.values = this.values.filter(v => v !== e.target.value);
      }
      this.$emit("input", this.values);
    }
  }
};
</script>

<style scoped></style>

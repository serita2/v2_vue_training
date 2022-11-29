<template>
  <validation-provider rules="required" name="name" v-slot={errors}>
  <input
    :type="type"
    :name="name"
    :value="value"
    :placeholder="placeholder"
    @input="updateValue"
    class="nameInput"
  /><br>
  <span class="error">{{ errors[0] }}</span>
  </validation-provider>
</template>

<script>
import { ValidationProvider, extend, localize } from "vee-validate";
import { required } from 'vee-validate/dist/rules';
import ja from "vee-validate/dist/locale/ja.json";

extend('required', required);
localize('ja', ja);
localize({
  ja: {
    names: {
      name: "名前",
    },
   },
});

export default {
  name: "MyInput",
  components: {
    ValidationProvider
  },
  props: {
    value: { type: String, require: true },
    type: { type: String, require: true },
    name: { type: String, require: true },
    placeholder: { type: String, require: false }
  },
  methods: {
    updateValue: function(e) {
      this.$emit("input", e.target.value);
    }
  }
};
</script>

<style scoped lang="scss">
.error{
  color:red;
  font-size: small;
}
</style>

<template>
  <input :value="modelValue" :type="type" :placeholder="placeholder" :class="inputClass" :v-mask="mask"
    @input="updateValue" />
</template>

<script>
export default {
  name: 'InputField',
  props: {
    modelValue: {
      type: [String, Number],
      default: '',
    },
    type: {
      type: String,
      default: 'text',
    },
    placeholder: {
      type: String,
      default: '',
    },
    inputClass: {
      type: String,
      default: 'input',
    },
    mask: {
      type: String,
      default: '',
    },
  },
  methods: {
    updateValue(event) {
      this.$emit('update:modelValue', event.target.value);
    },
  },
};
</script>

<style>
.input {
  width: 100%;
  padding: 12px;
  border: 1px solid #ccc;
  border-radius: 6px;
  font-size: 1rem;
  outline: none;
  cursor: pointer;
  transition: border-color 0.3s ease, box-shadow 0.3s ease;
}

.input:focus {
  border-color: var(--focus-border-color);
  box-shadow: var(--focus-box-shadow);
  outline: none;
}
</style>
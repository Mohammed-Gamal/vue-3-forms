<template>
  <label v-if="label">{{ label }}</label>
  <input
    v-model="value"
    v-bind="$attrs"
    :placeholder="label"
    class="field"
  >
</template>

<script>
export default {
  props: {
    label: {
      type: String,
      default: ''
    },
    modelValue: {
      type: [String, Number],
      required: true,
      default: ''
    }
  },
  emits: ['update:modelValue'],
  computed: {
    value: {
      get() {
        return this.modelValue
      },
      set(value) {
        this.$emit('update:modelValue', value)
      }
    }
  },
  inheritAttrs: false,
}
</script>

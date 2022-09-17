<template>
  <input
    v-bind="$attrs"
    type="checkbox"
    :checked="modelValue"
    @change="$emit('update:modelValue', $event.target.checked)"
    class="field"
  />
  <label v-if="label">{{ label }}</label>
</template>

<script>
export default {
  inheritAttrs: false,
  props: {
    label: {
      type: String,
      default: ''
    },
    modelValue: {
      type: Boolean,
      default: false
    }
  },
  emits: ['update:modelValue']
}
</script>
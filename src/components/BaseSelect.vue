<template>
  <label v-if="label">{{ label }}</label>
  <select
    class="field"
    :value="modelValue"
    v-bind="{
      ...$attrs,
      onChange: ($event) => $emit('update:modelValue', $event.target.value)
    }"
  >
    <option
      v-for="option in options"
      :value="option"
      :key="option"
      :selected="option === modelValue"
    >{{ option }}</option>
  </select>
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
      type: String,
      required: true,
      default: ''
    },
    options: {
      type: Array,
      required: true,
      default: () => []
    }
  }
}
</script>

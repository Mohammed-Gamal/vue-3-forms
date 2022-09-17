<template>
  <component
    v-for="option in options"
    :key="option.value"
    :is="vertical ? 'div' : 'span'"
    :class="{ horizontal: !vertical }"
  >
    <BaseRadio
      :label="option.label"
      :value="option.value"
      :name="name"
      v-model.number="value"
    />
  </component>
</template>

<script>
export default {
  props: {
    options: {
      type: Array,
      required: true
    },
    modelValue: {
      type: [String, Number],
      default: ''
    },
    name: {
      type: String,
      default: ''
    },
    vertical: {
      type: Boolean,
      default: false
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
  }
}
</script>

<style scoped>
  .horizontal {
    margin-right: 20px;
  }
</style>

<template>
  <label v-if="label" :for="uuid">{{ label }}</label>
  <input
    v-model="value"
    v-bind="$attrs"
    :placeholder="label"
    :id="uuid"
    class="field"
    :aria-describedby="error ? `${uuid}-error` : null"
    :aria-invalid="error ? true : null"
  >
  <!-- aria-live, alternative for role="alert" -->
  <p
    v-if="error"
    :id="`${uuid}-error`"
    class="errorMessage"
    aria-live="assertive"
  >
    {{ error }}
  </p>
</template>

<script>
import UniqueID from '@/features/UniqueID'

export default {
  inheritAttrs: false,
  props: {
    label: {
      type: String,
      default: ''
    },
    modelValue: {
      type: [String, Number],
      required: true,
      default: ''
    },
    error: {
      type: String,
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
  setup() {
    const uuid = UniqueID().getID()

    return { uuid }
  }
}
</script>

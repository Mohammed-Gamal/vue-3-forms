<template>
  <form @submit.prevent="submit">
    <BaseInput
      label="Email"
      type="email"
      :modelValue="email"
      @change="handleChange"
      :error="errors.email"
      :class="{ error: errors.email }"
    />

    <BaseInput
      label="Password"
      type="password"
      v-model="password"
      :error="errors.password"
      :class="{ error: errors.password }"
    />

    <BaseButton class="-fill-gradient">Submit</BaseButton>
  </form>
</template>

<script>
import { useField, useForm } from 'vee-validate'
import { object, string } from 'yup'

export default {
  setup () {
    // Validations Schema
    const validationSchema = object({
      email: string().required().email(),
      password: string().required().min(5)
    })

    const { handleSubmit, errors, setFieldValue } = useForm({
      validationSchema
    })

    const { value: email } = useField('email')
    const { value: password } = useField('password')

    const handleChange = (event) => {
      setFieldValue('email', event.target.value)
    }

    const submit = handleSubmit(values => {
      console.log('Submit', values)
    })

    return {
      email,
      password,
      submit,
      errors,
      handleChange
    }
  }
}
</script>

<template>
  <div>
    <h1>Create an event</h1>

    <form @submit.prevent="submit">
      <BaseSelect
        label="Select a category"
        :options="categories"
        v-model="category"
        :error="errors.category"
      />

      <fieldset>
        <legend>Name & describe your event</legend>

        <BaseInput
          label="Title"
          type="text"
          v-model="title"
          :error="errors.title"
        />

        <BaseInput
          label="Description"
          type="text"
          v-model="description"
          :error="errors.description"
        />
      </fieldset>

      <fieldset>
        <legend>Where is your event?</legend>

        <BaseInput
          label="Location"
          type="text"
          v-model="location"
          :error="errors.location"
        />
      </fieldset>

      <fieldset>
        <legend>Pets</legend>

        <p>Are pets allowed?</p>

        <div>
          <BaseRadioGroup
            name="pets"
            :options="petOptions"
            v-model="pets"
            :error="errors.pets"
          />
        </div>
      </fieldset>

      <fieldset>
        <legend>Extras</legend>

        <div>
          <BaseCheckbox
            label="Catering"
            v-model="catering"
            :error="errors.catering"
          />
        </div>

        <div>
          <BaseCheckbox
            label="Live music"
            v-model="music"
            :error="errors.music"
          />
        </div>
      </fieldset>

      <BaseButton class="button -fill-gradient">Submit</BaseButton>
    </form>
  </div>
</template>

<script>
import axios from 'axios'
import { useField, useForm } from 'vee-validate'

export default {
    data() {
      return {
        categories: [
          "sustainability",
          "nature",
          "animal welfare",
          "housing",
          "education",
          "food",
          "community"
        ],
        petOptions: [
          {label: 'Yes', value: 1},
          {label: 'No', value: 0}
        ]
      };
    },
    setup() {
      const required = value => {
        const requiredMessage = 'This field is required!'

        if (value === undefined || value === null) return requiredMessage
        if (!String(value).length) return requiredMessage

        return true
      }

      const minLength = (number, value) => {
        if (String(value).length < number) return `This field must contain at least ${number} characters!`

        return true
      }

      const anything = () => {
        return true
      }

      const validationSchema = {
        category: required,
        title: value => {
          const req = required(value)
          if (req !== true) return req

          const min = minLength(3, value)
          if (min !== true) return min

          return true
        },
        description: required,
        location: undefined,
        pets: anything,
        catering: anything,
        music: anything
      }

      const { handleSubmit, errors } = useForm({
        validationSchema,
        initialValues: {
          pets: 1,
          catering: false,
          music: false
        }
      })

      const { value: category }     =   useField('category')
      const { value: title }        =   useField('title')
      const { value: description }  =   useField('description')
      const { value: location }     =   useField('location')
      const { value: pets }         =   useField('pets')
      const { value: catering }     =   useField('catering')
      const { value: music }        =   useField('music')

      const submit = handleSubmit(values => {
        axios.post(
          `http://localhost:3000/events`,
          {
            "category": category.value,
            "title": title.value,
            "description": description.value,
            "location": location.value,
            "pets": pets.value,
            "extras": {
              "catering": catering.value,
              "music": music.value
            }
          }
        )
        .then(res => {
          console.log('Response:', res)
        })
        .catch(err => {
          console.log('Error', err)
        })
      })

      return {
        category,
        title,
        description,
        location,
        pets,
        catering,
        music,
        submit,
        errors
      }
    }
}
</script>

<style>
fieldset {
  border: 0;
  margin: 0;
  padding: 0;
}

legend {
  font-size: 28px;
  font-weight: 700;
  margin-top: 20px;
}
</style>

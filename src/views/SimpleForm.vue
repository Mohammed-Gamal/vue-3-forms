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
import { object, string, number, boolean } from 'yup'

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
      const validationSchema = object({
        category: string().required(),
        title: string().required('A cool title is required').min(3).lazy,
        description: string().required(),
        location: string(),
        pets: number(),
        catering: boolean(),
        music: boolean()
      })

      const { handleSubmit, errors } = useForm({
        validationSchema,
        initialValues: {
          pets: 1,
          catering: false,
          music: false
        }
      })

      const { value: category    }   =   useField('category')
      const { value: title       }   =   useField('title')
      const { value: description }   =   useField('description')
      const { value: location    }   =   useField('location')
      const { value: pets        }   =   useField('pets')
      const { value: catering    }   =   useField('catering')
      const { value: music       }   =   useField('music')

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

<style scoped>
fieldset {
  border: 0;
  padding: 0;
  margin-bottom: 20px;
}

legend {
  font-size: 28px;
  font-weight: 700;
  margin-top: 20px;
}
</style>

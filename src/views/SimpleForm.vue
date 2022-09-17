<template>
  <div>
    <h1>Create an event</h1>

    <form @submit.prevent="sendForm">
      <BaseSelect
        label="Select a category"
        :options="categories"
        v-model="event.category"
      />

      <h3>Name & describe your event</h3>

      <BaseInput
        label="Title"
        v-model="event.title"
        type="text"
      />

      <BaseInput
        label="Description"
        v-model="event.description"
        type="text"
      />

      <h3>Where is your event?</h3>

      <BaseInput
        label="Location"
        v-model="event.location"
        type="text"
      />

      <h3>Are pets allowed?</h3>
      <div>
        <BaseRadioGroup
          v-model="event.pets"
          name="pets"
          :options="petOptions"
        />
      </div>

      <h3>Extras</h3>

      <div>
        <BaseCheckbox
          v-model="event.extras.catering"
          label="Catering"
        />
      </div>

      <div>
        <BaseCheckbox
          v-model="event.extras.music"
          label="Live music"
        />
      </div>

      <button class="button -fill-gradient">Submit</button>
    </form>
  </div>
</template>

<script>
import axios from 'axios'

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
        event: {
          category: "",
          title: "",
          description: "",
          location: "",
          pets: 1,
          extras: {
              catering: false,
              music: false
          }
        },
        petOptions: [
          {label: 'Yes', value: 1},
          {label: 'No', value: 0}
        ]
      };
    },
    methods: {
      sendForm() {
        axios.post(
          `https://my-json-server.typicode.com/Mohammed-Gamal/vue-3-forms`, this.event
        )
          .then(response => {
            console.log('Response', response)
          })
          .catch(error => {
            console.log('Error:', error)
          })
      }
    }
}
</script>

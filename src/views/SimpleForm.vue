<template>
  <div>
    <h1>Create an event</h1>

    <form @submit.prevent="sendForm">
      <BaseSelect
        label="Select a category"
        :options="categories"
        v-model="event.category"
      />

      <fieldset>
        <legend>Name & describe your event</legend>

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
      </fieldset>

      <fieldset>
        <legend>Where is your event?</legend>

        <BaseInput
          label="Location"
          v-model="event.location"
          type="text"
        />
      </fieldset>

      <fieldset>
        <legend>Pets</legend>

        <p>Are pets allowed?</p>

        <div>
          <BaseRadioGroup
            v-model="event.pets"
            name="pets"
            :options="petOptions"
          />
        </div>
      </fieldset>

      <fieldset>
        <legend>Extras</legend>

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
      </fieldset>

      <BaseButton class="button -fill-gradient">Submit</BaseButton>
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
          `http://localhost:3000/events`,
          this.event
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

<template>
  <section class="container">
    <b-button
      class="main-button"
      variant="primary"
      type="button"
      @click="showForm = !showForm">Add Food Item
    </b-button>
    <article
      v-if="showForm"
      class="form-container">
      <form v-if="showForm">
        <b-form
          v-if="show"
          @submit="onSubmit">
          <b-form-group
            id="foodName"
            label="Food Name:"
            label-for="foodName">
            <b-form-input
              id="foodName"
              v-model="form.name"
              type="text"
              required
              placeholder="Enter food"/>
          </b-form-group>
          <b-form-group
            id="foodCategory"
            label="Category:"
            label-for="foodCategory">
            <b-form-select
              id="foodCategory"
              :options="categories"
              v-model="form.type"
              required/>
          </b-form-group>
          <b-form-group
            id="date"
            label="Expiration Date:"
            label-for="date">
            <b-form-input 
              id="date"
              v-model="form.expDate"
              type="date"
              required
              placeholder="Expiration date"/>
          </b-form-group>
          <b-button
            id="submit"
            type="submit"
            variant="success">Submit Food Item
          </b-button>
        </b-form>
      </form>
    </article>
    <b-button
      class="main-button"
      type="button"
      variant="secondary"
      @click="showImage = !showImage">Expiration Guide
    </b-button>
    <img
      v-if="showImage"
      class="guide-img"
      src="https://thumbnails-visually.netdna-ssl.com/TheShelfLifeofFood_512f96cbed064.jpg"
      alt="an image that shows the expiration date for many types of foods">
  </section>
</template>

<script>
export default {
  props: {
    getFood: {
      type: Function,
      required: true
    },
    userId: {
      type: Number,
      required: true
    }
  },
  data() {
    return {
      id: this.userId,
      showForm: false,
      showImage: false,
      form: {
        user_id: this.userId,
        name: '',
        type: '',
        expDate: null
      },
      categories: [
        { text: 'Select One', value: null },
        'Produce', 'Dairy', 'Meat', 'Grain', 'Other'
      ],
      show: true
    }
  },
  methods: {
    onSubmit() {
      const data = this.form
      fetch('https://g-spoiler-alert.herokuapp.com/api/v1/pantry/newitem', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify(data),
        mode: 'cors',
      })
      .then(res => res.json())
      .then(res => console.log(res))
      .then(this.getFood())
      .then(this.showForm = false)
    },
  },
}
</script>

<style scoped>
section {
  display: flex;
  flex-direction: column;
  margin-top: 2%;
  align-items:center;
}

.form-container {
  background-color: rgba(0, 0, 0, 0.5);
  width: inherit;
  padding-bottom: 5%;
  border-radius: .25rem;
  padding-top: 0.375rem;
  padding-right: 0.75rem;
  padding-bottom: 0.375rem;
  padding-left: 0.75rem;
}

.guide-img {
  width: 50%;
  height: auto;
  margin-bottom: 10%;
}

.main-button {
  width: 50%;
  margin: 1em;
}

b-form-group[label="Food Name:"] {
  background-color: rgba(255, 255, 255, 0.5)
}
</style>

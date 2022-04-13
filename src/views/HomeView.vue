<script>
import axios from "axios";

export default {
  data: function () {
    return {
      places: [],
      newPlaceParams: {},
      errors: [],
      place: {},
      currentPlace: {},
    };
  },
  created: function () {
    axios.get("http://localhost:3000/places").then((response) => {
      this.places = response.data;
      console.log(this.places);
    });
  },
  methods: {
    createPlace: function () {
      axios
        .post("http://localhost:3000/places", this.newPlaceParams)
        .then((response) => {
          console.log("Success", response.data);
          this.places.push(response.data);
        })
        .catch((error) => console.log(error.response));
    },
    showPlace: function (place) {
      console.log(place);
      this.currentPlace = place;
      document.querySelector("#place-details").showModal();
    },
  },
};
</script>

<template>
  <h1>Places</h1>
  <p>
    Location:
    <input type="text" v-model="newPlaceParams.name" />
    Address:
    <input type="text" v-model="newPlaceParams.address" />
  </p>
  <button v-on:click="createPlace()">Create</button>
  <div v-for="place in places" :key="place">
    <p>{{ place.name }}</p>
    <button v-on:click="showPlace(place)">Address</button>
  </div>
  <dialog id="place-details">
    <form method="dialog">
      <h1>{{ currentPlace.name }}</h1>
      <p>{{ currentPlace.address }}</p>
      <button>Close</button>
    </form>
  </dialog>
</template>

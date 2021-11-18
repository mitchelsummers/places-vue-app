<template>
  <div class="home">
    <h1>New Place</h1>
    <p>Title:</p>
    <input type="string" v-model="newPlaceParams.name" />
    <p>Plot:</p>
    <input type="text" v-model="newPlaceParams.address" />
    <button v-on:click="createPlace()">Create Place</button>

    <h1>All Places</h1>

    <div v-for="place in places" :key="place.id">
      <h2>{{ place.name }}</h2>
      <button v-on:click="showPlace(Place)">Place Details</button>
      <div>-----------------------------------------------</div>
      <dialog id="place-details">
        <form method="dialog">
          <h4>
            Name:
            <input type="text" v-model="currentPlace.name" />
          </h4>
          <p>
            Address:
            <input type="text" v-model="currentPlace.address" />
          </p>
          <button v-on:click="updatePlace(currentPlace)">Update Place</button>
          <button v-on:click="destroyPlace(currentPlace)">Delete Place</button>
          <button>close</button>
        </form>
      </dialog>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data: function () {
    return {
      places: [],
      newPlaceParams: {},
      currentPlace: {},
    };
  },
  created: function () {
    this.indexPlaces();
  },
  methods: {
    indexPlaces: function () {
      axios.get("http://localhost:3000/places").then((response) => {
        this.Places = response.data;
        console.log("All Places", this.places);
      });
    },
    createPlace: function () {
      axios.post("http://localhost:3000/places", this.newPlaceParams).then((response) => {
        console.log(response.data);
        this.Places.push(response.data);
      });
      this.newPlaceParams.name = "";
      this.newPlaceParams.address = "";
    },
    showPlace: function (place) {
      this.currentPlace = place;
      console.log(place);
      document.querySelector("#place-details").showModal();
    },
    updatePlace: function (place) {
      axios.patch("http://localhost:3000/Places/" + place.id, place).then((response) => {
        console.log("Place Created", response.data);
      });
    },
    destroyPlace: function (place) {
      axios.delete("http://localhost:3000/Places/" + place.id).then((response) => {
        console.log("Place Deleted", response.data);
        var index = this.places.indexOf(place);
        this.places.splice(index, 1);
      });
    },
  },
};
</script>

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
    axios.get("/places").then((response) => {
      console.log("all places", response.data);
      this.places = response.data;
    });
  },
  methods: {
    createPlace: function () {
      axios.post("/places", this.newPlaceParams).then((response) => {
        console.log("place created!", response);
        this.places.push(response.data);
        this.newPlaceParams = {};
      });
    },
    showPlace: function (place) {
      this.currentPlace = place;
      document.querySelector("#place-details").showModal();
    },
    editPlace: function (place) {
      var editPlaceParams = place;
      axios.patch("/places/" + place.id, editPlaceParams).then((response) => {
        console.log("updated", response.data);
      });
    },
    destroyPlace: function (place) {
      axios.delete("/places/" + place.id).then((response) => {
        console.log("deleted", response.data);
        var index = this.places.indexOf(place);
        this.places.splice(index, 1);
      });
    },
  },
};
</script>

<template>
  <div class="home">
    <h1>All your places!</h1>

    <p>
      Name :
      <input type="text" v-model="this.newPlaceParams.name" />
    </p>
    <p>
      Address :
      <input type="text" v-model="this.newPlaceParams.address" />
    </p>
    <button v-on:click="createPlace()">Add Place!</button>
  </div>

  <div v-for="place in places" :key="place.id">
    <p>Name: {{ place.name }}</p>
    <p>Address: {{ place.address }}</p>
    <button v-on:click="showPlace(place)">Show more</button>
  </div>

  <div>
    <dialog id="place-details">
      <form method="dialog">
        <h4>Place info</h4>
        <p>
          Name :
          <input type="text" v-model="currentPlace.name" />
        </p>
        <p>
          Address :
          <input type="text" v-model="currentPlace.address" />
        </p>
        <button>Close</button>
        <button v-on:click="editPlace(currentPlace)">Update</button>
        <button v-on:click="destroyPlace(currentPlace)">Delete</button>
      </form>
    </dialog>
  </div>
</template>

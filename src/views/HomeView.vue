<script>
import axios from "axios";

export default {
  data: function () {
    return {
      places: [],
      newPlaceParams: {},
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
  },
};
</script>

<template>
  <div class="home">
    <h1>All your places!</h1>

    <form action="">
      <p>
        Name :
        <input type="text" v-model="this.newPlaceParams.name" />
      </p>
      <p>
        Address :
        <input type="text" v-model="this.newPlaceParams.address" />
      </p>
      <button v-on:click="createPlace()">Add Place!</button>
    </form>
  </div>

  <div v-for="place in places" :key="place.id">
    <p>Name: {{ place.name }}</p>
    <p>Address: {{ place.address }}</p>
  </div>
</template>

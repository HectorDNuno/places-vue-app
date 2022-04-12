<script>
import axios from "axios";

export default {
  data: function () {
    return {
      places: {},
    };
  },
  created: function () {
    axios.get("http://localhost:3000/places").then((response) => {
      console.log("all places", response.data);
      this.places = response.data;
    });
  },
  methods: {},
};
</script>

<template>
  <div class="home">
    <h1>All your places!</h1>

    <div v-for="place in places" :key="place.id">
      <p>Name: {{ place.name }}</p>
      <p>Address: {{ place.address }}</p>
    </div>
  </div>
</template>

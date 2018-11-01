<template>
  <div class="home">
    <form v-on:submit.prevent="submit()">
      <h2>Create a new Profile</h2>
      <div>
        Name: <input type="text" v-model="name">
      </div>
      <div>
        Image: <input type="file" v-on:change="setFile($event)" ref="fileInput">
      </div>
      <input type="submit" value="Go">
    </form>
    <div>
      <div v-for="profile in profiles">
        <h2>{{ profile.name }}</h2>
        <img v-bind:src="profile.image_url">
      </div>
    </div>
  </div>
</template>

<style>
</style>

<script>
  var axios = require('axios');

export default {
  data: function() {
    return {
      profiles: [],
      name: "",
      image: ""
    };
  },
  created: function() {
    axios
    .get("http://localhost:3000/api/profiles")
    .then(response => {
      this.profiles = response.data;
    });
  },
  methods: {
    submit: function() {
      var formData = new FormData();
      formData.append("name", this.name);
      formData.append("image", this.image);

      axios
      .post("http://localhost:3000/api/profiles", formData)
      .then(response => {
        this.name = "";
        this.$refs.fileInput.value = "";

        this.profiles = response.data;
      });
    },
    setFile: function(event) {
      if (event.target.files.length > 0) {
        this.image = event.target.files[0];
      }
    }
  },
  computed: {}
};
</script>


















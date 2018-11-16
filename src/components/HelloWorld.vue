<style>
.myContent {
  padding-top: 20px;
}

img {
  padding: 5px;
}
</style>


<template>
    <div class="container">
        <h1 class="title">{{ msg }}</h1>

        <input name="search" class="input" v-model="searchTerm">
        <br><br>
        <button @click="performSearch" class="button is-primary is-large">Search</button>

        <div class="myContent">
            <img v-for="i in images" :key="i.id" :src="i.images.fixed_height_still.url">
        </div>
    </div>
</template>

<script>
import axios from "axios";
import cloudinary from "cloudinary-core";

export default {
  name: "HelloWorld",
  data() {
    return {
      msg: "Welcome to Image Search Manipulator",
      searchTerm: "dogs",
      images: [],
      manipulatedImages: ""
    };
  },
  methods: {
    performSearch: function() {
      // console.log(this.searchTerm);
      var link = "http://api.giphy.com/v1/gifs/search?api_key=dc6zaTOxFJmzC&q=";
      const apiLink = link + this.searchTerm;

      const cl = new cloudinary.Cloudinary({
        cloud_name: "nikola"
      });

      axios
        .get(apiLink)
          .then(response => {
            // console.log(response);
            this.images = response.data.data;

            this.images.forEach(image => {
              const cloudinaryImage = cl
                .imageTag(image.images.fixed_height_still.url, {
                  width: 150,
                  height: 150
                })
                .toHtml();

              this.manipulatedImages += cloudinaryImage;
            });
          })
          .catch(error => {
              console.log(error);
          });
    }
  }
};
</script>

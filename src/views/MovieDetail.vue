<script setup>
import { ref, onBeforeMount } from "vue";
import { useRoute } from "vue-router";
// import env from "@/env.js";

const movie = ref({});
const route = useRoute();

onBeforeMount(() => {
  fetch(
    `https://www.eporner.com/api/v2/video/id/?id=${route.params.id}&thumbsize=medium&format=json`
  )
    .then((response) => response.json())
    .then((data) => {
      movie.value = data;
      console.log(movie.value);
    });
});
</script>
<template>
  <div class="movie-detail">
    <div class="vedio-wrapper">
      <iframe :src="movie.embed" frameborder="0" allowfullscreen></iframe>
    </div>
    <h2>{{ movie.title }}</h2>
    <p>{{ movie.Year }}</p>
    <img :src="movie.Poster" alt="Movie Poster" class="featured-img" />
    <p>{{ movie.Plot }}</p>
  </div>
</template>
<style>
.movie-detail {
  padding: 16px;

  h2 {
    color: #fff;
    font-size: 28px;
    font-weight: 600;
    margin-bottom: 16px;
  }

  .featured-img {
    display: block;
    max-width: 200px;
    margin-bottom: 16px;
  }

  p {
    color: #fff;
    font-size: 18px;
    line-height: 1.4;
  }

  .vedio-wrapper {
  
  }
  .vedio-wrapper iframe {
    width: 100%;
    height: 700px;
    margin-left: auto;
    margin-right: auto;
  }
}
</style>

<script setup>
import { ref } from "vue";
// import env from "@/env.js";

const search = ref("");
const movies = ref([]);

var pageCount = ref(2)

function SearchMovies() {
  if (search.value != "") {
    fetch(
      `https://www.eporner.com/api/v2/video/search/?query=${search.value}&per_page=10&page=${pageCount.value}&thumbsize=big&order=top-weekly&gay=1&lq=1&format=json`
    )
      .then((response) => response.json())
      .then((data) => {
        movies.value = data.videos;
        // search.value = "";
        console.log(movies.value);
      });
  }
}

function clickNext() {
  SearchMovies();
  pageCount.value ++;
}

function clickBack() {
  if(pageCount.value >= 2){
    SearchMovies();
    pageCount.value --;
  }
}

</script>
<template>
  <div class="home">
    <div class="feature-card">
      <router-link to="/movie/tt0409591">
        <img
          src="https://static-ca-cdn.eporner.com/thumbs/static4/1/10/104/10401157/1_360.jpg"
          alt="Naruto Poster"
          class="featured-img"
        />
        <div class="detail">
          <h3>Naruto</h3>
          <p>
            Naruto Uzumaki, a mischievous adolescent ninja, struggles as he
            searches for recognition and dreams of becoming the Hokage, the
            village's leader and strongest ninja.
          </p>
        </div>
      </router-link>
    </div>

    <form @submit.prevent="SearchMovies()" class="search-box">
      <input
        type="text"
        v-model="search"
        placeholder="What are you looking for..?"
      />
      <input type="submit" value="Search" />
    </form>

    <div class="movies-list">
      <div class="movie" v-for="movie in movies" :key="movie.id">
        <router-link :to="'/movie/' + movie.id">
          <div class="movie-link">
            <div class="product-image">
              <img :src="movie.default_thumb.src" alt="Movie Poster" />
              <div class="type"></div>
            </div>
            <div class="detail">
              <p class="year">Min:{{ movie.length_min }}</p>
              <p class="year">Views:{{ movie.views }}</p>
              <p class="year">{{ movie.title }}</p>
            </div>
          </div>
        </router-link>
      </div>
      <div class="page">
        <button @click="clickBack">Back</button>
        <p>{{ }}</p>
        <button @click="clickNext">Next</button>
      </div>
    </div>
  </div>
</template>
<style>
.home {
  .feature-card {
    position: relative;

    .featured-img {
      display: block;
      width: 100%;
      height: 300px;
      object-fit: cover;

      position: relative;
      z-index: 0;
    }

    .detail {
      position: absolute;
      left: 0;
      right: 0;
      bottom: 0;
      background-color: rgba(0, 0, 0, 0.6);
      padding: 16px;
      z-index: 1;

      h3 {
        color: #fff;
        margin-bottom: 16px;
      }

      p {
        color: #fff;
      }
    }
  }

  .search-box {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    padding: 16px;

    input {
      display: block;
      appearance: none;
      border: none;
      outline: none;
      background: none;

      &[type="text"] {
        width: 100%;
        color: #fff;
        background-color: #496583;
        font-size: 20px;
        padding: 10px 16px;
        border-radius: 8px;
        margin-bottom: 15px;
        transition: 0.4s;

        &::placeholder {
          color: #f3f3f3;
        }

        &:focus {
          box-shadow: 0px 3px 6px rgba(0, 0, 0, 0.2);
        }
      }

      &[type="submit"] {
        width: 100%;
        max-width: 300px;
        background-color: #42b883;
        padding: 16px;
        border-radius: 8px;
        color: #fff;
        font-size: 20px;
        text-transform: uppercase;
        transition: 0.4s;

        &:active {
          background-color: #3b8070;
        }
      }
    }
  }

  .movies-list {
    display: flex;
    flex-wrap: wrap;
    margin: 0px 8px;

    .movie {
      padding: 16px 8px;
      flex: 1 1 100%;

      @media (min-width: 768px) {
        max-width: 33.33%;
        flex: 1 1 33.33%;
      }

      .movie-link {
        display: flex;
        flex-direction: column;
        height: 100%;

        .product-image {
          position: relative;
          display: block;

          img {
            display: block;
            width: 100%;
            height: 275px;
            object-fit: cover;
          }

          .type {
            position: absolute;
            padding: 8px 16px;
            background-color: #42b883;
            color: #fff;
            bottom: 16px;
            left: 0px;
            text-transform: capitalize;
          }
        }

        .detail {
          background-color: #496583;
          padding: 16px 8px;
          flex: 1 1 100%;
          border-radius: 0px 0px 8px 8px;

          .year {
            color: #aaa;
            font-size: 14px;
            text-align: center;
          }

          h3 {
            color: #fff;
            font-weight: 600;
            font-size: 18px;
          }
        }
      }
    }
  }
}

.page {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 10px;
  margin: 10px;
  width: 100%;
  margin-left: auto;
  margin-right: auto;
}

.page button {
  width: 100px;
  height: 40px;
  font-size: 18px;
  font-weight: bold;
  background-color: #42b883;
  cursor: pointer;
}
.page button:hover {
  background-color: #0f6324; /* Change to the desired color when hovering */
}
</style>

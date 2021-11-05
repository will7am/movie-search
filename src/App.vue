<template>
  <div class="container">
    <div class="row">
      <div class="col-md-3"></div>
      <div class="col-md-5">
        <div class="input-group input-group-lg">
          <div class="input-group-prepend">
            <span class="input-group-text">
              <img src="@/assets/image/search.png" class="icon" />
            </span>
          </div>
          <input
            type="text"
            class="form-control"
            placeholder="Search Movie Here"
            @keyup.enter="showMovie"
            v-model="search_words"
          />
        </div>
      </div>
      <button
        type="button"
        class="btn btn-custom col-md-1"
        v-on:click="showMovie"
      >
        Search
      </button>
    </div>
    <br />
    <div v-if="movies" class="row">
      <div v-for="movie in movie_array" :key="movie.show.id" class="col-md-4">
        <div v-if="movie.show.image == null">
          <Movie :src="url" :id="'collapse' + movie.show.id"
            >{{ movie.show.name }}
            <template v-slot:des>
              <p v-html="movie.show.summary"></p>
            </template>
          </Movie>
        </div>
        <div v-else>
          <Movie
            :src="movie.show.image.original"
            :id="'collapse' + movie.show.id"
            >{{ movie.show.name }}
            <template v-slot:des>
              <p v-html="movie.show.summary"></p>
            </template>
          </Movie>
        </div>
      </div>
    </div>
    <div v-if="movies.length" class="row">
      <span class="col-md-4 custom_text text-left">{{ movies.length }} results</span>
      <button
        type="button"
        class="btn btn-white rounded show_more col-md-4 border-dark"
        v-on:click="showMore"
      >
        Show me more results
      </button>
    </div>
    <div class="footer"></div>
  </div>
</template>

<script>
import Movie from "./components/Movie.vue";

export default {
  name: "App",
  components: {
    Movie,
  },
  data() {
    return {
      counter: 3,
      movies: [],
      movie_array: [],
      search_words: [],
      url: "https://cdn.pixabay.com/photo/2014/04/03/11/07/clapperboard-311792_960_720.png",
    };
  },
  methods: {
    async getData() {
      try {
        let response = await fetch(
          "https://api.tvmaze.com/search/shows?q=" + this.search_words
        );
        this.movies = await response.json();
        this.movie_array = this.movies.slice(0, this.counter);
      } catch (error) {
        console.log(error);
      }
    },
    showMovie: function () {
      this.counter = 3;
      this.movies = [];
      this.movie_array = [];
      this.getData();
    },

    showMore: function () {
      this.counter += 3;
      this.getData();
      // if (this.counter > this.movies.length) this.counter = this.movies.length;
    },
  },
};
</script>

<style>
#app {
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Oxygen,
    Ubuntu, Cantarell, "Open Sans", "Helvetica Neue", sans-serif Pro Display,
    SF Pro Icons, Helvetica Neue, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
@import "./assets/css/custom.css";
</style>

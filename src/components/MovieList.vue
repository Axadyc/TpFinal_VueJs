<template>
    <v-card>
      <v-card-title class="text-h4 mb-4">Bienvenue</v-card-title>
      <v-row justify="center">
        <v-col v-for="(movie, index) in movies" :key="index" cols="12" sm="6" md="4" lg="3">
          <v-card>
            <v-img :src="getImageUrl(movie.poster_path)" aspect-ratio="2.5"></v-img>
            <v-card-title class="text-h5">{{ movie.title }}</v-card-title>
            <v-card-actions>
              <v-btn @click="showMovieDetail(movie)">Plus d'infos</v-btn>
            </v-card-actions>
          </v-card>
        </v-col>
      </v-row>
    </v-card>
  </template>
  
  <script>
  import axios from 'axios';
  import config from '../config.json';
  
  export default {
    name: 'MoviesList',
    data() {
      return {
        movies: [],
        photoPath: config.url.photo_path
      };
    },
    created() {
      this.getMovies();
    },
    methods: {
      getMovies() {
        axios.get(config.url.movie_list, {
          params: {
            include_adult: false,
            include_video: false,
            language: 'fr-FR',
            sort_by: 'popularity.desc',
            api_key: config.api_key
          }
        })
        .then(res => {
          this.movies = res.data.results;
        })
        .catch(error => {
          console.log(error);
        });
      },
      getImageUrl(posterPath) {
        return this.photoPath + posterPath;
      },
      showMovieDetail(movie) {
        this.$emit('showMovieDetailEmit', movie);
      }
    }
  };
  </script>
  
  <style scoped>
  .text-h4 {
    font-size: 24px;
    font-weight: bold;
  }
  .text-h5 {
    font-size: 20px;
    font-weight: bold;
  }
  .mb-4 {
    margin-bottom: 16px;
  }
  </style>
  
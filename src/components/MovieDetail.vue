<template>
    <v-dialog v-model="dialog">
      <v-card>
        <v-row align="center">
          <v-col cols="12" md="6">
            <v-img :src="photoUrl" max-width="300" height="auto" contain></v-img>
          </v-col>
          <v-col cols="12" md="6">
            <v-card-title class="text-h4">{{ movieDetails.title }}</v-card-title>
            <v-card-text>
              <div class="text-h5 mb-4">{{ movieDetails.overview }}</div>
              <div>Sociétés de production :</div>
              <v-row>
                <v-col v-for="(company, index) in movieDetails.production_companies" :key="index" cols="12">
                  <v-chip>{{ company.name }}</v-chip>
                </v-col>
              </v-row>
              <div class="mt-4">Date de sortie : {{ movieDetails.release_date }}</div>
              <div>Note moyenne : {{ movieDetails.vote_average }}/10</div>
            </v-card-text>
          </v-col>
        </v-row>
        <v-divider></v-divider>
        <v-card-actions>
          <v-btn color="primary" @click="closeDetail">Fermer</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </template>
  
  <script>
  import axios from 'axios';
  import config from '../config.json';
  
  export default {
    name: 'MovieDetail',
    props: {
      movieId: {
        type: Number,
        required: true
      }
    },
    data() {
      return {
        dialog: true,
        movieDetails: {},
        photoUrl: ''
      };
    },
    methods: {
      closeDetail() {
        this.$emit('closeMovieDetailEmit'); 
      },
      fetchMovieDetails() {
        axios.get(`${config.url.movie_detail}${this.movieId}`, {
          params: {
            api_key: config.api_key
          }
        })
        .then(response => {
          this.movieDetails = response.data;
          this.photoUrl = `${config.url.photo_path}${this.movieDetails.poster_path}`;
        })
        .catch(error => {
          console.error('Erreur lors de la récupération des détails du film :', error);
        });
      }
    },
    mounted() {
      this.fetchMovieDetails();
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
  </style>
  
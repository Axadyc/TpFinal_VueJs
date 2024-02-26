<template>
  <div>
    <movieSearch @searchMovieEmit="setSearchMovie"></movieSearch>
    <MovieList @showMovieDetailEmit="showMovieDetail" :movies="movieList" :recherche="this.recherche"></MovieList>
    <v-dialog v-model="visible">
      <MovieDetail @closeMovieDetailEmit="closeMovieDetail" v-if="visible===true" :movieId="selectedMovieId"></MovieDetail>
    </v-dialog>
  </div>
</template>

<script>
import MovieDetail from '@/components/MovieDetail.vue';
import MovieList from '@/components/MovieList.vue';
import movieSearch from '@/components/movieSearch.vue';

import axios from 'axios';
import config from '../config.json';

export default {
  name: 'HomeView',
  components: { MovieList, MovieDetail, movieSearch },
  data(){
    return{
      visible : false,
      selectedMovieId : null,
      recherche : "",
      movieList: []
    }
  },
  methods:{
    showMovieDetail(selectedMovie){
      this.selectedMovieId = selectedMovie.id;
      this.visible = true;
    },
    closeMovieDetail(){
      this.visible = false;
    },
    setSearchMovie(val){
      this.recherche = val;
    },
    fetchMovies() {
      axios.get(config.url.movie_list, {
        headers: {
          'Authorization': `token ${config.token}`
        }
      })
      .then(res => {
        this.movieList = res.data.results;
      })
      .catch(error => {
        console.log(error);
      });
    }
  },
  mounted() {
    this.fetchMovies();
  }
}
</script>

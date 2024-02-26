<template>
    <div>
        <data value="img">this.image</data>
    </div>
</template>
<script>

import axios from 'axios';
import config from '..\config.json';

export default {
    name: 'MoviesList',
    methods: {
        getMovies() {
        axios.get(config.url.movie_list, {
            headers: {
                'Authorization': `token ${config.token}`
            }
        })
        .then(response => {
            console.log(response.data);
            image = config.photo_path
            
        })
        .catch(error => {
            console.log(error);
        })
        }
    }
}

</script>
<template>
  <div>
    <HomeView></HomeView>
  </div>
</template>

<script>
import HomeView from './views/HomeView.vue';

export default {
    name: 'App',
    components: { HomeView }
}
</script>

<style>
.v-card, .v-img {
  text-align: center;
}
</style>

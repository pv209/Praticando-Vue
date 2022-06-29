<template>
  <div class="root">
    <v-app-bar
      app
      color="primary"
      dark
    >
    </v-app-bar>
    <MainPage/>
    </div>



</template>

<script>
import MainPage from './components/MainPage.vue';

export default {
  name: 'App',

  components: {
    MainPage,
  },

  data: () => ({
    //
  }),
};
</script>

<style scoped>
 .root{
   display: flex;
   flex-direction: column ;
   align-content: space-around;
 }
</style>

<template>
  <v-app>
    <v-app-bar
      app
      color="black"
      dark
    >
      <div class="d-flex align-center">
        <h1>Teste JukeBox</h1>
      </div>

      

    </v-app-bar>

    <v-main>
      <home/>
    </v-main>
  </v-app>
</template>

<script>
import home from './components/index';

export default {
  name: 'App',

  components: {
    home,
  },

  data: () => ({
    //
  }),
};
</script>

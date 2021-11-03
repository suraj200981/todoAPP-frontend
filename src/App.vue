<template>
  <v-app> 
 <v-app-bar
      color="deep-purple"
      dark
    >

      <v-toolbar-title>TodoAPP v1 by Suraj Sharma</v-toolbar-title>

    </v-app-bar>

    <v-main>
      <br>
      <center>
      <mainForms/>
      </center>
    </v-main>
  </v-app>
</template>

<script>
import mainForms from './components/mainForms';

export default {
  name: 'App',

  components: {
    mainForms,
  },

  data: () => ({
    //
  }),
};
</script>



<template>
  <v-app>
    <v-app-bar app>
      <v-toolbar-title class="headline text-uppercase">
        <span>kinspector</span> -
        <span class="font-weight-light">Check the details of any PGP key</span>
      </v-toolbar-title>
      <v-spacer></v-spacer>
      <v-btn text href="https://github.com/dethos/kinspector" target="_blank">
        <span class="mr-2">Git Repository</span>
      </v-btn>
    </v-app-bar>

    <v-content>
      <KeyInspector />
    </v-content>
  </v-app>
</template>

<script>
import KeyInspector from "./components/KeyInspector";

export default {
  name: "App",
  components: {
    KeyInspector
  },
  data: () => ({}),
  created: function() {
    this.$vuetify.theme.dark = true;
  }
};
</script>

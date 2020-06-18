<template>
  <v-app>
    <v-app-bar app>
      <v-toolbar-title class="headline text-uppercase">
        <span>kinspect</span> -
        <span class="font-weight-light">Check the details of any PGP key</span>
      </v-toolbar-title>
      <v-spacer></v-spacer>
      <v-btn text @click.stop="searchDialog = true">
        <span class="mr-2">Search Key</span>
      </v-btn>
      <v-btn text v-on:click="changeTheme">
        <span class="mr-2">Swicth to {{darkTheme ? "light theme" : "dark theme"}}</span>
      </v-btn>
      <v-btn text href="https://github.com/dethos/kinspect" target="_blank">
        <span class="mr-2">Check source code</span>
      </v-btn>
    </v-app-bar>

    <v-content>
      <KeyInspect v-bind:initialKey="searchKey" />
      <DirSearch
        v-bind:dialog="searchDialog"
        v-on:dialogClosed="closeDialog"
        v-on:keyFound="setSearchKey"
      />
    </v-content>

    <v-footer>
      <div class="flex-grow-1"></div>
      <div>
        Developed by
        <a href="https://ovalerio.net>">Gonçalo Valério</a> | License:
        <a href="https://github.com/dethos/kinspect/blob/master/LICENSE">AGPL</a>
      </div>
    </v-footer>
  </v-app>
</template>

<script>
import KeyInspect from "./components/KeyInspect";
import DirSearch from "./components/DirSearch";

export default {
  name: "App",
  components: {
    KeyInspect,
    DirSearch
  },
  data: () => ({
    darkTheme: true,
    searchDialog: false,
    searchKey: ""
  }),
  created: function() {
    this.$vuetify.theme.dark = this.darkTheme;
  },
  methods: {
    changeTheme: function() {
      this.darkTheme = !this.darkTheme;
      this.$vuetify.theme.dark = this.darkTheme;
    },
    closeDialog: function() {
      this.searchDialog = false;
    },
    setSearchKey: function(armoredKey) {
      this.searchKey = armoredKey;
    }
  }
};
</script>

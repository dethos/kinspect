<template>
  <v-container fluid>
    <v-row>
      <v-col>
        <v-textarea
          outlined
          autofocus
          auto-grow
          rows="10"
          height="100%"
          hint="Paste your public key here"
          label="Public Key"
        ></v-textarea>
      </v-col>
      <v-col>
        <KeyDetails />
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import KeyDetails from "./KeyDetails";

export default {
  components: {
    KeyDetails
  },
  data: () => ({})
};
</script>

<style lang="stylus"></style>

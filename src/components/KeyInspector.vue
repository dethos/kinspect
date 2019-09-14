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
          hint="Paste your public key here. Click outside of the textarea to trigger the evaluation."
          label="Public Key"
          v-on:change="inspect"
          v-model="pubkey"
        ></v-textarea>
      </v-col>
      <v-col>
        <KeyDetails v-bind:fingerprint="fingerprint" />
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import KeyDetails from "./KeyDetails";
import * as openpgp from "openpgp";

export default {
  components: {
    KeyDetails
  },
  data: () => ({
    pubkey: "",
    fingerprint: ""
  }),
  methods: {
    inspect: async function() {
      let keys = (await openpgp.key.readArmored(this.pubkey)).keys;
      this.fingerprint = keys[0].getFingerprint();
    }
  }
};
</script>

<style lang="stylus"></style>

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
        <h1 class="font-weight-light">Key Details</h1>
        <div v-if="error">
          <h1>{{error}}</h1>
        </div>
        <div v-for="pgpkey in keys" v-bind:key="pgpkey.getFingerprint()">
          <KeyDetails v-bind:pgpkey="pgpkey" />
        </div>
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
    keys: [],
    error: ""
  }),
  methods: {
    inspect: async function() {
      try {
        this.keys = (await openpgp.key.readArmored(this.pubkey)).keys;
        this.error = "";
      } catch (e) {
        this.keys = [];
        this.error = "Unable to parse the provided key";
      }
    }
  }
};
</script>

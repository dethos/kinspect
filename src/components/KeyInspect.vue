<template>
  <v-container fluid>
    <v-row>
      <v-col>
        <v-alert
          v-model="alert"
          dismissible
          type="error"
        >Do not paste any private key that is currently in use.</v-alert>
      </v-col>
    </v-row>
    <v-row>
      <v-col>
        <v-textarea
          outlined
          autofocus
          rows="15"
          hint="Paste your public key here. Click outside of the textarea to trigger the evaluation."
          label="Public Key"
          v-on:change="inspect"
          v-model="pubkey"
        ></v-textarea>
      </v-col>
    </v-row>

    <v-row v-if="error">
      <v-col>
        <h1>{{ error }}</h1>
      </v-col>
    </v-row>

    <v-row v-for="pgpkey in keys" v-bind:key="pgpkey.getFingerprint()">
      <v-col>
        <KeyDetails v-bind:pgpkey="pgpkey" />
      </v-col>
      <v-col v-for="subkey in pgpkey.subKeys" v-bind:key="subkey.getFingerprint()">
        <SubKey v-bind:pgpkey="subkey" />
      </v-col>
      <v-col v-for="user in pgpkey.users" v-bind:key="user.userId.userid">
        <UserId v-bind:user="user" />
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import KeyDetails from "./KeyDetails";
import SubKey from "./SubKey";
import UserId from "./UserId";
import * as openpgp from "openpgp";

export default {
  components: {
    KeyDetails,
    SubKey,
    UserId
  },
  data: () => ({
    pubkey: "",
    keys: [],
    error: "",
    alert: localStorage.getItem("keyAlertDismissed") == "true" ? false : true
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
  },
  watch: {
    alert: function() {
      localStorage.setItem("keyAlertDismissed", true);
    }
  }
};
</script>

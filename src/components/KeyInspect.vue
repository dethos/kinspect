<template>
  <v-container fluid>
    <v-row v-if="alert">
      <v-col>
        <v-alert
          v-model="alert"
          dismissible
          type="error"
        >Do not paste any private key that is currently in use.</v-alert>
      </v-col>
    </v-row>
    <v-row>
      <v-col cols="3" v-if="previousPubKeys.length > 0">
        <KeyList v-bind:keys="previousPubKeys" v-on:viewKey="setKey" />
      </v-col>
      <v-col>
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

        <v-row v-if="key">
          <v-col>
            <KeyDetails v-bind:pgpkey="key" />
          </v-col>
          <v-col v-for="subkey in key.subKeys" v-bind:key="subkey.getFingerprint()">
            <SubKey v-bind:pgpkey="subkey" />
          </v-col>
          <v-col v-for="user in key.users" v-bind:key="user.userId.userid">
            <UserId v-bind:user="user" />
          </v-col>
        </v-row>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import KeyList from "./KeyList";
import KeyDetails from "./KeyDetails";
import SubKey from "./SubKey";
import UserId from "./UserId";
import * as openpgp from "openpgp";

export default {
  components: {
    KeyList,
    KeyDetails,
    SubKey,
    UserId
  },
  data: () => ({
    pubkey: "",
    key: null,
    error: "",
    alert: localStorage.getItem("keyAlertDismissed") == "true" ? false : true,
    previousPubKeys: JSON.parse(localStorage.getItem("previousPubKeys")) || []
  }),
  methods: {
    inspect: async function() {
      try {
        this.key = (await openpgp.key.readArmored(this.pubkey)).keys[0];
        this.error = "";
      } catch (e) {
        this.key = null;
        this.error = "Unable to parse the provided key";
      }

      if (this.key) {
        this.updateKeyList();
      }
    },
    updateKeyList: async function() {
      let fingerprint = this.key.getFingerprint().toUpperCase();
      let user = (await this.key.getPrimaryUser()).user.userId;
      let found = this.previousPubKeys.find(
        key => key.fingerprint == fingerprint
      );
      if (!found) {
        this.previousPubKeys.unshift({
          fingerprint: fingerprint,
          created: this.key.getCreationTime(),
          added: new Date().toISOString(),
          email: user.email,
          pubkey: this.pubkey
        });
        localStorage.setItem(
          "previousPubKeys",
          JSON.stringify(this.previousPubKeys)
        );
      }
    },
    setKey: function(key) {
      this.pubkey = key;
      this.inspect();
    }
  },
  watch: {
    alert: function() {
      localStorage.setItem("keyAlertDismissed", true);
    }
  }
};
</script>

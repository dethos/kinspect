<template>
  <v-dialog v-model="dialog" width="500">
    <v-card>
      <v-card-title class="headline" primary-title>Key Search</v-card-title>
      <v-card-text>
        <p>
          Find a public key based on a given email address. Kinspect will do a
          <a
            href="https://wiki.gnupg.org/WKD"
          >
            Web Key Directory
            (WKD)
          </a> lookup to find and select the correct public key.
        </p>
        <v-text-field v-model="email" :rules="emailRules" label="Email Address" required></v-text-field>
        <p v-if="error">{{error}}</p>
      </v-card-text>
      <v-divider></v-divider>
      <v-card-actions>
        <v-spacer></v-spacer>
        <v-btn color="primary" text @click="searchKey()">Search</v-btn>
        <v-btn color="primary" text @click="closeDialog()">Close</v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>

<script>
import WKD from "openpgp";

export default {
  props: ["dialog"],
  data: () => ({
    email: "",
    emailRules: [
      (v) => !!v || "E-mail is required",
      (v) => /.+@.+\..+/.test(v) || "E-mail must be valid",
    ],
    error: "",
  }),
  methods: {
    closeDialog: function () {
      this.$emit("dialogClosed");
    },
    searchKey: async function () {
      this.error = "";
      let wkd = new WKD();
      try {
        let result = await wkd.lookup({ email: this.email });
        let key = result.keys[0];
        this.$emit("keyFound", key.armor());
        this.$emit("dialogClosed");
      } catch (e) {
        this.error = "Unable to find the key for the specified email address.";
      }
    },
  },
};
</script>

<template>
  <v-card>
    <v-card-title>
      <v-icon>mdi-key</v-icon>Subkey
      <v-chip class="ma-2" outlined label>
        {{
        pgpkey
        .getKeyId()
        .toHex()
        .toUpperCase()
        }}
      </v-chip>
      <div class="flex-grow-1"></div>
      <v-chip class="ma-2" color="red" text-color="white" label v-if="revoked">Revoked</v-chip>
      <v-chip class="ma-2" color="orange" text-color="white" label v-if="expired">Expired</v-chip>
    </v-card-title>
    <v-divider />
    <v-card-text>
      <v-list>
        <v-list-item-content>
          <v-list-item-title>
            <strong>Fingerprint:</strong>
            {{ pgpkey.getFingerprint().toUpperCase() }}
          </v-list-item-title>
        </v-list-item-content>
        <v-divider />
        <v-list-item-content>
          <v-list-item-title>
            <strong>Algorithm:</strong>
            {{ pgpkey.getAlgorithmInfo()["algorithm"] }}
            <v-chip
              class="ma-2"
              color="primary"
              label
              text-color="white"
              v-if="pgpkey.getAlgorithmInfo()['bits']"
            >{{ pgpkey.getAlgorithmInfo()["bits"] }} bits</v-chip>
            <v-chip
              class="ma-2"
              color="primary"
              label
              text-color="white"
              v-if="pgpkey.getAlgorithmInfo()['curve']"
            >{{ pgpkey.getAlgorithmInfo()["curve"]}}</v-chip>
          </v-list-item-title>
        </v-list-item-content>
        <v-divider />
        <v-list-item-content>
          <v-list-item-title>
            <strong>Created:</strong>
            {{ pgpkey.getCreationTime() }}
          </v-list-item-title>
        </v-list-item-content>
        <v-divider />
        <v-list-item-content>
          <v-list-item-title>
            <strong>Expires:</strong>
            {{ expirationDate }}
          </v-list-item-title>
        </v-list-item-content>
      </v-list>
    </v-card-text>
  </v-card>
</template>

<script>
export default {
  props: ["pgpkey"],
  data: () => ({
    expirationDate: "",
    expired: false,
    revoked: false
  }),
  watch: {
    pgpkey: function() {
      this.refreshData();
    }
  },
  created: function() {
    this.refreshData();
  },
  methods: {
    getExpirationDate: async function() {
      let expirationDate = await this.pgpkey.getExpirationTime();
      if (expirationDate) {
        this.expirationDate = expirationDate;
        this.expired = new Date(this.expirationDate) < new Date();
      } else {
        this.expirationDate = "Never";
      }
    },
    is_revoked: async function() {
      this.revoked = await this.pgpkey.isRevoked();
    },
    refreshData: function() {
      this.getExpirationDate();
      this.is_revoked();
    }
  }
};
</script>

<template>
  <v-card>
    <v-card-title>
      <span v-if="pgpkey.isPublic()">Public Key</span>
      <span v-if="pgpkey.isPrivate()">Private Key</span>
      <v-chip class="ma-2" outlined label>{{pgpkey.getKeyId().toHex().toUpperCase()}}</v-chip>
      <div class="flex-grow-1"></div>
      <v-chip class="ma-2" color="red" text-color="white" label v-if="revoked">Revoked</v-chip>
      <v-chip class="ma-2" color="orange" text-color="white" label v-if="expired">Expired</v-chip>
    </v-card-title>
    <v-divider />
    <v-card-text>
      <v-subheader>General</v-subheader>
      <v-list>
        <v-list-item-content>
          <v-list-item-title>
            <strong>Fingerprint:</strong>
            {{pgpkey.getFingerprint().toUpperCase()}}
          </v-list-item-title>
        </v-list-item-content>
        <v-divider />
        <v-list-item-content>
          <v-list-item-title>
            <strong>Algorithm:</strong>
            {{pgpkey.getAlgorithmInfo()["algorithm"]}} (Size: {{pgpkey.getAlgorithmInfo()["bits"]}})
          </v-list-item-title>
        </v-list-item-content>
        <v-divider />
        <v-list-item-content>
          <v-list-item-title>
            <strong>Created:</strong>
            {{pgpkey.getCreationTime()}}
          </v-list-item-title>
        </v-list-item-content>
        <v-divider />
        <v-list-item-content>
          <v-list-item-title>
            <strong>Expires:</strong>
            {{expirationDate}}
          </v-list-item-title>
        </v-list-item-content>
      </v-list>
      <v-list>
        <v-subheader>User details</v-subheader>
      </v-list>
      <div>
        <v-subheader>Subkeys</v-subheader>
      </div>
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
  created: function() {
    this.getExpirationDate();
    this.is_revoked();
  },
  methods: {
    getExpirationDate: async function() {
      this.expirationDate = await this.pgpkey.getExpirationTime();
      this.expired = new Date(this.expirationDate) < new Date();
    },
    is_revoked: async function() {
      this.revoked = await this.pgpkey.isRevoked();
    }
  }
};
</script>

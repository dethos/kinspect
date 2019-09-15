<template>
  <v-card>
    <v-card-title>
      <span v-if="pgpkey.isPublic()">Public</span>
      <span v-if="pgpkey.isPrivate()">Private</span>
      Key: {{pgpkey.getKeyId().toHex().toUpperCase()}}
    </v-card-title>
    <v-card-text>
      <div>
        <strong>Fingerprint:</strong>
        {{pgpkey.getFingerprint().toUpperCase()}}
      </div>
      <div>
        <strong>Algorithm:</strong>
        {{pgpkey.getAlgorithmInfo()["algorithm"]}} (Size: {{pgpkey.getAlgorithmInfo()["bits"]}})
      </div>
      <div>
        <strong>Created:</strong>
        {{pgpkey.getCreationTime()}}
      </div>
      <div>
        <strong>Expires:</strong>
        {{expirationDate}}
      </div>
      <div>
        <strong>Revoked:</strong>
        <span v-if="revoked">Yes</span>
        <span v-if="!revoked">No</span>
      </div>
    </v-card-text>
  </v-card>
</template>

<script>
export default {
  props: ["pgpkey"],
  data: () => ({
    expirationDate: "",
    revoked: false
  }),
  created: function() {
    this.getExpirationDate();
    this.is_revoked();
  },
  methods: {
    getExpirationDate: async function() {
      this.expirationDate = await this.pgpkey.getExpirationTime();
    },
    is_revoked: async function() {
      this.revoked = await this.pgpkey.isRevoked();
    }
  }
};
</script>

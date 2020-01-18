<template>
  <v-card>
    <v-card-title>
      <v-icon>mdi-account</v-icon>
    </v-card-title>
    <v-divider />
    <v-card-text>
      <v-list>
        <v-list-item-content>
          <v-list-item-title>
            <strong>Name:</strong>
            {{ user.userId.name }}
          </v-list-item-title>
        </v-list-item-content>
        <v-divider />
        <v-list-item-content>
          <v-list-item-title>
            <strong>Email:</strong>
            {{ user.userId.email }}
          </v-list-item-title>
        </v-list-item-content>
        <v-divider />
        <v-list-item-content>
          <v-list-item-title>
            <strong>Comment:</strong>
            {{ user.userId.comment }}
          </v-list-item-title>
        </v-list-item-content>
      </v-list>
    </v-card-text>
  </v-card>
</template>

<script>
export default {
  props: ["user"]
};
</script>

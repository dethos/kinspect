<template>
  <v-card>
    <v-card-title>Key Details</v-card-title>
    <v-card-text>{{fingerprint}}</v-card-text>
  </v-card>
</template>

<script>
export default {
  props: ["fingerprint"]
};
</script>


<style>
</style>

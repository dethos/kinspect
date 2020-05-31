<template>
  <v-card>
    <v-toolbar dark>
      <v-toolbar-title>Previous Keys</v-toolbar-title>
      <v-spacer></v-spacer>
      <v-btn icon @click.native="clearAllKeys">
        <v-icon>mdi-checkbox-marked-circle</v-icon>
      </v-btn>
    </v-toolbar>
    <v-list two-line>
      <v-list-item-group>
        <template v-for="(item, index) in keys">
          <v-list-item :key="item.fingerprint" @click="selectKey(index)">
            <template>
              <v-list-item-content>
                <v-list-item-title v-text="item.name"></v-list-item-title>
                <v-list-item-subtitle class="text--primary" v-text="item.email"></v-list-item-subtitle>
                <v-list-item-subtitle v-text="item.fingerprint"></v-list-item-subtitle>
              </v-list-item-content>

              <v-list-item-action>
                <v-list-item-action-text v-text="item.added"></v-list-item-action-text>
              </v-list-item-action>
            </template>
          </v-list-item>

          <v-divider v-if="index + 1 < keys.length" :key="index"></v-divider>
        </template>
      </v-list-item-group>
    </v-list>
  </v-card>
</template>

<script>
export default {
  props: ["keys"],
  methods: {
    selectKey: function(index) {
      this.$emit("viewKey", this.keys[index].pubkey);
    },
    clearAllKeys: function() {
      this.$emit("clearKeys");
    }
  }
};
</script>

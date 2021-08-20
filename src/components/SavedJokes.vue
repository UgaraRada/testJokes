<template>
  <v-main>
    <v-expansion-panels class="mb-6">
      <v-expansion-panel>
        <v-expansion-panel-header class="font-weight-bold">
          Saved jokes
        </v-expansion-panel-header>
        <v-expansion-panel-content
          v-for="it in savedJokes"
          :key="it.id"
        >
          {{ it.joke }}
        </v-expansion-panel-content>
      </v-expansion-panel>
    </v-expansion-panels>
  </v-main>
</template>

<script>
export default {
  name: 'SavedJokes',
  props: {
    savedJokes: Array,
  },
};
</script>

<style>

</style>

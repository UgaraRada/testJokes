<template>
  <v-expansion-panels class="mb-6">
    <v-expansion-panel>
      <v-expansion-panel-header class="font-weight-bold">
        Saved jokes
      </v-expansion-panel-header>
      <v-expansion-panel-content
        v-for="it in savedJokes"
        :key="it.id"
      >
        <div class="joke joke--saved">
          <p class="joke--text">
            {{ it.joke }}
          </p>
          <button
            @click="$emit('removeJokeFromSaved', it)"
          >
            <v-icon class="button_hover">
              mdi-close
            </v-icon>
          </button>
        </div>
      </v-expansion-panel-content>
    </v-expansion-panel>
  </v-expansion-panels>
</template>

<script>
export default {
  name: 'SavedJokes',
  props: {
    savedJokes: Array,
  },
};
</script>

<style scoped lang="scss">

.joke {

  &--text {
    line-height: 2rem;
  }
}

.button_hover:hover {
  color: rgb(238, 132, 132);
}
</style>

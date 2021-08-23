<template>
  <v-main>
    <v-container>
      <v-text-field
        v-model="search"
        placeholder="Enter a word to search for an anecdote"
        class="pa-4"
      />
      <saved-jokes
        :saved-jokes="savedJokes"
        @removeJokeFromSaved="removeJokeFromSaved"
      />
      <v-card>
        <section v-if="error">
          <p>
            We're sorry, we're not able to retrieve this
            information at the moment, please try back later
          </p>
        </section>
        <div v-else>
          <div
            v-if="loading"
            class="pa-1"
          >
            Loading...
          </div>
          <div
            v-for="it in filteredJokes"
            :key="it.id"
            class="joke joke--main"
            :class="it.active && 'active'"
          >
            <p class="joke--text">
              {{ it.joke }}
            </p>
            <v-icon
              :color="it.active? 'blue' : ''"
              @click="toggleLike(it)"
            >
              {{ it.active ? 'mdi-thumb-up' : 'mdi-thumb-up-outline' }}
            </v-icon>
          </div>
        </div>
      </v-card>
    </v-container>
  </v-main>
</template>

<script>
import axios from 'axios';
import SavedJokes from './SavedJokes.vue';

export default {
  name: 'JokePage',
  components: { SavedJokes },

  data() {
    return {
      jokes: [],
      search: '',
      error: false,
      loading: true,
      savedJokes: JSON.parse(localStorage.getItem('savedJokes')) || [],
    };
  },

  computed: {
    filteredJokes() {
      return this.jokes.filter((it) => it.joke.toLowerCase().includes(this.search.toLowerCase()));
    },

  },

  mounted() {
    axios.get('https://v2.jokeapi.dev/joke/Any?type=single&amount=10')
      .then((response) => {
        this.jokes = response.data.jokes.map((it) => ({ ...it, active: false }));
      }).catch(() => {
        this.error = true;
      }).finally(() => {
        this.loading = false;
      });
  },
  methods: {
    toggleLike(it) {
      it.active = !it.active;

      if (it.active) {
        this.addJokeToSaved(it);
      } else {
        this.removeJokeFromSaved(it);
      }
    },
    addJokeToSaved(it) {
      if (this.savedJokes.some((joke) => joke.id === it.id)) {
        return;
      }
      this.savedJokes.push(it);
      this.updateLocalStorage();
    },
    removeJokeFromSaved(it) {
      this.savedJokes = this.savedJokes.filter((joke) => joke.id !== it.id);
      it.active = false;
      this.updateLocalStorage();
    },
    updateLocalStorage() {
      localStorage.setItem('savedJokes', JSON.stringify(this.savedJokes));
    },
  },
};

</script>

<style scoped lang="scss">

.joke {
  padding: 20px;
  border-bottom: 1px solid rgba(0, 0, 0, 0.2);

  &.active {
    background-color: rgb(224 237 255);
  }

}
</style>

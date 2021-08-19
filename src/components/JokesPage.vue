<template>
  <v-main>
    <v-container>
      <v-text-field
        v-model="search"
        placeholder="Enter a word to search for anecdote"
        class="pa-4"
      />
      <v-expansion-panels class="mb-4">
        <v-expansion-panel
          v-for="(item,i) in 1"
          :key="i"
        >
          <v-expansion-panel-header>
            Saved jokes
          </v-expansion-panel-header>
          <v-expansion-panel-content
            v-for="it in savedJokes"
            :key="it"
          >
            {{ it }}
          </v-expansion-panel-content>
        </v-expansion-panel>
      </v-expansion-panels>

      <v-card>
        <section v-if="error">
          <p>
            We're sorry, we're not able to retrieve this
            information at the moment, please try back later
          </p>
        </section>
        <section v-else>
          <div
            v-if="loading"
            class="pa-1"
          >
            Loading...
          </div>
          <div
            v-for="it in filteredJokes"
            :key="it.id"
            :class="it.active && 'active'"
          >
            <div class="wrapper">
              <p class="joke-text">
                {{ it.joke }}
              </p>
              <v-icon @click="it.active = !it.active, addNewJoke(it.joke)">
                {{ it.active ? 'mdi-thumb-up' : 'mdi-thumb-up-outline' }}
              </v-icon>
            </div>
          </div>
        </section>
      </v-card>
    </v-container>
  </v-main>
</template>

<script>
import axios from 'axios';

export default {
  name: 'JokePage',

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
  watch: {
    savedJokes: {
      handler() {
        localStorage.setItem('savedJokes', JSON.stringify(this.savedJokes));
      },
      deep: true,
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
    addNewJoke(joke) {
      const set = new Set(this.savedJokes);
      set.add(joke);
      this.savedJokes = [...set];
    },
  },
};

</script>

<style scoped lang="scss">
.active {
  background-color: rgb(194, 209, 233);
}
.wrapper {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 20px;
  border-bottom: 1px solid black;
}
.joke-text {
  margin-bottom: 0;
  margin-right: 30px;
}
</style>

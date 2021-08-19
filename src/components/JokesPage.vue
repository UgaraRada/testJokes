<template>
  <v-main>
    <v-container>
      <v-text-field
        v-model="search"
        placeholder="Enter a word to search for anecdote"
        class="pa-4"
      />
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
              <v-icon @click="it.active = !it.active">
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

  },
};

</script>

<style scoped lang="scss">
.active {
  background-color: rgb(178, 200, 233);
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

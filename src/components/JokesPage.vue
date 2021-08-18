<template>
  <v-main>
    <v-container>
      <v-text-field
        v-model="searchWord"
        placeholder="Enter a word to search for anecdote"
      />
      <v-card>
        <section v-if="errored">
          <p>
            We're sorry, we're not able to retrieve this
            information at the moment, please try back later
          </p>
        </section>
        <section v-else>
          <div v-if="loading">
            Loading...
          </div>
          <div
            v-for="joke in filterArrJokes"
            :key="joke"
          >
            <p> {{ joke }} </p>
            <input type="checkbox">
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
      info: null,
      searchWord: '',
      errored: false,
      loading: true,
    };
  },

  computed: {
    arrJokes() {
      return this.info.map((it) => it.joke);
    },
    filterArrJokes() {
      return this.arrJokes.filter((it) => it
        .toString().toUpperCase().includes(this.searchWord.toUpperCase()));
    },
  },

  mounted() {
    axios
      .get('https://v2.jokeapi.dev/joke/Any?type=single&amount=10')
      .then((response) => {
        this.info = response.data.jokes;
      })
      .catch((error) => {
        console.log(error);
        this.errored = true;
      })
      .finally(() => { (this.loading = false); });
  },
};

</script>

<style scoped lang="scss">

</style>

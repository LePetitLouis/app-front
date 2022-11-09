<template>
  <div>
    <Header @search="searchFilm = $event" />
    <main class="container">
      <h1 class="mt-4 mb-4">Les dernières bandes-annonces</h1>
      <b-container v-if="films.length" class="bv-example-row">
        <b-row>
          <b-col v-for="(film, index) in filterFilm" :key="index" cols="4">
            <CardFilm
              :id="film.id"
              :title="film.title"
              :image="film.image"
              :synopsis="film.synopsis"
              :rating="rating[index]"
            />
          </b-col>
        </b-row>
      </b-container>
    </main>
  </div>
</template>

<script lang="ts">
import axios from 'axios'
import Vue from 'vue'
import CardFilm from '~/components/CardFilm.vue'
import Header from '~/components/Header.vue'

interface Film {
  id: number
  title: string
  image: string
  url: string
  synopsis: string
}

export default Vue.extend({
  components: { CardFilm, Header },

  async asyncData() {
    const res = await axios.get(
      'http://localhost:8055/items/films?fields=id,title,image,synopsis'
    )
    return { films: res.data.data }
  },

  data() {
    return {
      films: [] as Film[],
      searchFilm: '',
      rating: [],
    }
  },

  computed: {
    filterFilm(): Film[] {
      return this.films.filter((film: Film) => {
        return film.title
          .toLocaleLowerCase()
          .includes(this.searchFilm.toLocaleLowerCase())
      })
    },
  },

  created() {
    this.$nuxt.$on('search', ($event: any) => this.handleSearchUpdate($event))
    this.fetchRateFilm()
  },

  methods: {
    handleSearchUpdate(e: string) {
      this.searchFilm = e
    },

    async fetchRateFilm() {
      const res = await axios.get(
        'http://localhost:8055/items/films?fields=comments.score'
      )
      const score = res.data.data.map((value: any) => {
        const initialValue = 0
        const total = value.comments.reduce(
          (accumulator: number, currentValue: any) =>
            accumulator + currentValue.score,
          initialValue
        )
        return total === 0 ? null : total / value.comments.length
      })
      this.rating = score
    },
  },
})
</script>

<style>
.container {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  text-align: center;
}

.title {
  font-family: 'Quicksand', 'Source Sans Pro', -apple-system, BlinkMacSystemFont,
    'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}
</style>

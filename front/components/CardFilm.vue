<template>
  <b-card
    :title="title"
    :img-src="baseUrl + image"
    img-alt="Image"
    img-top
    tag="article"
    style="max-width: 20rem"
    class="mb-2"
  >
    <p v-if="rating">Note moyenne: {{ rating }}</p>
    <b-card-text>
      {{ synopsis }}
    </b-card-text>

    <nuxt-link :to="'/film/' + id" variant="primary">En savoir plus</nuxt-link>
  </b-card>
</template>

<script>
export default {
  name: 'CardFilm',
  props: ['id', 'title', 'image', 'synopsis', 'rating'],
  data() {
    return {
      baseUrl: 'http://localhost:8055/assets/',
    }
  },
}
</script>

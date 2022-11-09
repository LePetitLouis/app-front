<template>
  <header>
    <b-navbar toggleable="lg" type="dark" variant="info">
      <b-navbar-brand href="/">Cinefile</b-navbar-brand>

      <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>

      <b-collapse id="nav-collapse" is-nav>
        <b-navbar-nav>
          <b-nav-item href="/">Accueil</b-nav-item>
        </b-navbar-nav>

        <b-navbar-nav class="ml-auto">
          <b-form-input
            v-model="searchFilm"
            size="sm"
            class="mr-sm-2"
            placeholder="Rechercher..."
          ></b-form-input>
          <b-button size="sm" class="my-2 my-sm-0" @click="emitValue"
            >Rechercher</b-button
          >
        </b-navbar-nav>
      </b-collapse>
    </b-navbar>
  </header>
</template>

<script>
export default {
  name: 'Header',
  data() {
    return {
      searchFilm: '',
    }
  },

  methods: {
    emitValue() {
      this.$nuxt.$emit('search', this.searchFilm)
    },
  },
}
</script>

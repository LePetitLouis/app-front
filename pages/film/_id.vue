<template>
  <div>
    <Header />
    <main class="container p-5">
      <b-row>
        <b-col style="margin: auto">
          <h1 class="mt-4">{{ film.title }}</h1>
          <h4>{{ $moment(film.releaseDate).format('LL') }}</h4>
          <section>
            <b-row style="justify-content: center">
              <b-col v-for="(tag, index) in film.type" :key="index" cols="auto">
                <b-badge variant="info">{{ tag.type_id.name }}</b-badge>
              </b-col>
            </b-row>
          </section>
        </b-col>

        <b-col>
          <iframe
            width="560"
            height="315"
            title="YouTube video player"
            :src="film.url"
            frameborder="0"
            allowfullscreen
            style="margin: 0 auto"
          ></iframe>
        </b-col>
      </b-row>

      <h2 class="mt-5" style="text-align: initial">Synopsis</h2>
      <p style="text-align: initial">{{ film.synopsis }}</p>

      <div v-if="comments.length" style="text-align: initial">
        <h2>Commentaires:</h2>
        <div v-for="(comment, index) in comments" :key="index">
          <Comment
            :username="comment.username"
            :comment="comment.comment"
            :score="comment.score"
          />
        </div>
      </div>

      <div>
        <b-button @click="modalShow = !modalShow"
          >Laisser un commentaire</b-button
        >

        <b-modal v-model="modalShow" title="Laisser un commentaire" hide-footer>
          <label for="rating-score">Note</label>
          <b-form-rating id="rating-score" v-model="form.score"></b-form-rating>

          <b-form-group
            id="input-group-username"
            label="Pseudo"
            label-for="input-username"
          >
            <b-form-input
              id="input-username"
              v-model="form.username"
              placeholder="Entrer votre pseudo"
              required
            >
            </b-form-input>
          </b-form-group>

          <b-form-group
            id="input-group-comment"
            label="Commentaire"
            label-for="input-comment"
          >
            <b-form-input
              id="input-comment"
              v-model="form.comment"
              placeholder="Entrer votre commentaire"
              required
            >
            </b-form-input>
          </b-form-group>

          <b-row>
            <b-col>
              <b-button class="mt-3" block variant="danger" @click="hideModal"
                >Annuler</b-button
              >
            </b-col>
            <b-col>
              <b-button
                class="mt-3"
                block
                variant="success"
                @click="sendComment"
                >Valider</b-button
              >
            </b-col>
          </b-row>
        </b-modal>
      </div>
    </main>
  </div>
</template>

<script lang="ts">
import axios from 'axios'
import Vue from 'vue'
import Comment from '~/components/Comment.vue'
import Header from '~/components/Header.vue'

interface Film {
  title: string
  image: string
  url: string
  synopsis: string
  type: any[]
  releaseDate: Date
}

interface FormComment {
  username: string
  comment: string
  score: number
}

export default Vue.extend({
  components: { Header, Comment },

  async asyncData({ params }) {
    const res = await axios.get(
      `http://localhost:8055/items/films/${params.id}?fields=title,image,url,synopsis,type.*.*`
    )
    return { film: res.data.data }
  },

  data() {
    return {
      film: {} as Film,
      modalShow: false,
      form: {} as FormComment,
      comments: [] as FormComment[],
      id: this.$route.params.id,
    }
  },

  created() {
    this.fetchComments()
    this.addViewForFilm()
  },

  methods: {
    async fetchComments() {
      const res = await axios.get(
        `http://localhost:8055/items/films/${this.id}?fields=comments.*.*`
      )
      this.comments = res.data.data.comments
    },

    hideModal() {
      this.modalShow = false
      this.form.comment = ''
      this.form.score = 0
      this.form.username = ''
    },

    async sendComment() {
      const body = {
        ...this.form,
        film: this.$route.params.id,
      }
      await axios.post('http://localhost:8055/items/comments', body)
      await this.fetchComments()
      this.hideModal()
    },

    async addViewForFilm() {
      // await axios.put('http://localhost:8055/items/film')
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

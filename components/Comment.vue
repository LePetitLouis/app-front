<template>
  <article class="comment-container">
    <div>
      <div class="avatar">{{ avatar }}</div>
    </div>
    <div>
      <h5 class="username">{{ username }} - {{ score }} étoile(s)</h5>
      <p class="comment">{{ comment }}</p>
    </div>
  </article>
</template>

<script>
export default {
  name: 'Comment',
  props: ['username', 'comment', 'score'],

  computed: {
    avatar() {
      const nameArray = this.username.split(' ')
      const initialsArray = []
      for (let i = 0; i < nameArray.length; i++) {
        initialsArray.push(nameArray[i][0])
      }
      return initialsArray.join('').toUpperCase()
    },
  },
}
</script>

<style>
.comment-container {
  display: flex;
  align-items: center;
  gap: 8px;
  margin-bottom: 30px;
}

.avatar {
  background-color: #e7e9ee;
  border-radius: 50%;
  width: 42px;
  height: 42px;
  text-align: center;
  -ms-flex-pack: center;
  justify-content: center;
  font-weight: 700;
  font-size: 14px;
  line-height: 42px;
  color: #383d3e;
}

.username {
  color: #383d3e;
  margin: 0;
}

.comment {
  color: #bcbcbc;
  margin: 0;
}
</style>

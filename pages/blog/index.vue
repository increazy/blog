<template>
  <main>
    <section v-if="posts" class="grid">
      <div class="header row bg-yellow">
        <h1 class="col-6 fg-light">Nossas<br />novidades</h1>
        <img src="~/assets/imgs/blog.jpg" alt="Blog" />
      </div>

      <div class="list">
        <new side="left" hover="yellow" type="blog" class="col-12 fg-dark" :amount="10" :list="posts" />
      </div>
    </section>
  </main>
</template>

<script>
import { mapMutations } from 'vuex'

export default {
  async asyncData({ $content, error }) {
    let posts
    try {
      posts = await $content('blog').fetch()
    } catch (e) {
      error({ message: 'Postagens não foram encontradas' })
    }
    return { posts }
  },
  created() {
    this.setClass('bg-yellow')
  },
  methods: {
    ...mapMutations({
      setClass: 'header/setClass',
    }),
  },
}
</script>

<style scoped>
.header {
  padding: 30px 0px 0px;
  justify-content: space-between;
}
.header img {
  width: 40%;
  align-self: flex-end;
}
h1 {
  align-self: center;
  margin-left: 30px;
}
.list {
  margin: 20px;
  width: calc(100% - 40px);
}
</style>

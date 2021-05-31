<template>
  <main>
    <section v-if="posts" class="grid">
      <div class="header row bg-pink">
        <img src="~/assets/imgs/code.jpg" alt="Code" />
        <h1 class="col-6 fg-light">Nossa<br />documentação</h1>
      </div>

      <div class="list">
        <new side="left" hover="pink" type="docs" class="col-12 fg-dark" :amount="10" :list="posts" />
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
      posts = await $content('docs').fetch()
    } catch (e) {
      error({ message: 'Documentos não foram encontrados' })
    }
    return { posts }
  },
  mounted() {
    this.setClass('bg-pink fg-light')
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
  padding-right: 30px;
  text-align: right;
}
.list {
  margin: 20px;
  width: calc(100% - 40px);
}
</style>

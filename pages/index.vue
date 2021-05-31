<template>
  <main class="grid">
    <section class="row news fg-dark bg-light bd-yellow">
      <nuxt-link to="/blog" class="col-6">
        <h2 class="col-12 fg-yellow">Novas<br />postagens</h2>
        <img src="~/assets/imgs/blog.jpg" alt="Blog" />
      </nuxt-link>

      <section class="col-6 grid list">
        <new side="left" hover="yellow" type="blog" class="col-12" :amount="3" />
      </section>
    </section>

    <section class="row news news--reverse fg-dark bg-light bd-pink">
      <section class="col-6 grid list">
        <new side="left" hover="pink" type="docs" class="col-12" :amount="3" />
      </section>

      <nuxt-link to="/docs" class="col-6">
        <h2 class="col-12 fg-pink">Novas<br />documentações</h2>
        <img src="~/assets/imgs/code.jpg" alt="Code" />
      </nuxt-link>
    </section>
  </main>
</template>

<script>
import { mapMutations } from 'vuex'

export default {
  mounted() {
    this.setClass('')
  },
  methods: {
    ...mapMutations({
      setClass: 'header/setClass',
    }),
  },
}
</script>
<style scoped>
main.grid {
  width: 100%;
  padding: 0px;
  margin: 0px;
}

.news {
  padding: 50px 0px 0px;
  border-bottom: 1px solid;
}

h2 {
  font-size: 36px;
  margin-left: 30px;
  line-height: 34px;
  margin-bottom: 20px;
  font-weight: 400;
}

.news--reverse h2 {
  margin-left: 0px;
  padding-right: 30px;
  text-align: right;
}

.list {
  padding: 0px 0px 0px 20px;
  align-items: flex-end;
}
.news--reverse .list {
  padding: 0px 20px 0px 0px;
}
</style>
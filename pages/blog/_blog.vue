<template>
  <main class="grid">
    <nav :class="colorClass" class="row heade fg-light" data-grid="space-between" aria-label="go back">
      <router-back />
      <span v-if="post.updatedAt" class="time">
        {{ formatDate(post.updatedAt) }}
      </span>
      <div class="tags col">
        <span v-for="tag in post.tags" :key="tag" :class="getTagClass(tag)" class="tag fg-light">
          {{ tag.trim() }}
        </span>
      </div>
    </nav>

    <article :class="colorClass" class="grid">
      <h1 class="col-12 fg-light title" data-grid="center">{{ post.title }}</h1>
      <p class="col-12 fg-light" data-grid="center">{{ post.description }}</p>
    </article>

    <nuxt-content :document="post" class="bg-light-alt post" />
  </main>
</template>

<script>
import { mapMutations } from 'vuex'

export default {
  async asyncData({ $content, params, error }) {
    let post
    try {
      post = await $content('blog', params.blog).fetch()
    } catch (e) {
      error({ message: 'Blog post not found' })
    }
    return { post }
  },
  data: () => ({
    colorClass: '',
  }),
  mounted() {
    const firstTag = Array.isArray(this.post.tags) ? this.post.tags[0] : this.post.tags
    this.colorClass = this.getTagClass(firstTag)
    this.setClass(this.colorClass)
  },
  methods: {
    formatDate(dateString) {
      const date = new Date(dateString)
      return date.toLocaleDateString(process.env.lang) || ''
    },
    getTagClass(tag) {
      const slug = tag.replace(' ', '-')
      if (slug.startsWith('increazy-')) {
        return `bg-${slug.replace('increazy-', '')}`
      }

      const avaliable = ['yellow', 'purple', 'orange', 'pink']

      return `bg-${avaliable[Math.floor(Math.random() * avaliable.length)]}`
    },
    ...mapMutations({
      setClass: 'header/setClass',
    }),
  },
}
</script>

<style scoped>
.header {
  padding: 10px 20px;
}

.tag:not(:last-child) {
  margin-right: 1px;
}

.title {
  padding: 70px 0px;
}

.post {
  width: calc(100% - 40px);
  margin: 20px;
  padding: 5px 10px;
}
</style>
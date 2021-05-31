<template>
  <ul v-if="posts.length > 0" :data-grid="side" class="grid news-container">
    <li v-for="(post, index) in posts" :key="index" :class="`fg-${hover}--hover`" class="new bg-light-alt">
      <nuxt-link :to="`${postType}/${post.slug}`" class="row">
        <h3 class="col-12" :data-grid="side">
          {{ post.title }}
        </h3>

        <p class="col-12" :data-grid="side">
          {{ post.description }}
        </p>

        <div class="meta col-12 row" :data-grid="side" :data-inverted="side">
          <span class="col">
            {{ formatDate(post.updatedAt) }}
          </span>
          <div class="tags col">
            <span v-for="tag in post.tags" :key="tag" :class="getTagClass(tag)" class="tag fg-light">
              {{ tag.trim() }}
            </span>
          </div>
        </div>
      </nuxt-link>
    </li>
  </ul>

  <div v-else-if="loading" class="grid">
    <div v-for="placeholder in placeholderClasses" :key="placeholder.id" class="new">
      <content-placeholders :rounded="true" :class="placeholder">
        <content-placeholders-heading />
      </content-placeholders>
    </div>
  </div>

  <p v-else class="grid" data-grid="center-center">Sem novidades no momento :c</p>
</template>

<script>
export default {
  props: {
    type: {
      type: String,
    },
    side: {
      type: String,
    },
    hover: {
      type: String,
    },
    amount: {
      type: Number,
      default: 10,
      validator: (val) => val >= 0 && val < 100,
    },
    sortBy: {
      type: Object,
      default: () => ({
        key: 'slug',
        direction: 'desc',
      }),
      validator: (obj) => typeof obj.key === 'string' && typeof obj.direction === 'string',
    },
  },
  data: () => ({
    loading: true,
    posts: [],
  }),
  computed: {
    placeholderClasses() {
      const classes = ['w-full', 'w-2/3', 'w-5/6']
      return [...Array.from({ length: this.amount }, (v, i) => classes[i % classes.length])] // repeats classes after one another
    },
  },
  async mounted() {
    this.loading = true
    this.posts = await this.fetchPosts()
    this.posts = this.posts.map((post) => {
      post.tags = Array.isArray(post.tags) ? post.tags : [post.tags]
      return post
    })
    this.loading = false
  },
  methods: {
    formatDate(dateString) {
      const date = new Date(dateString)
      return date.toLocaleDateString(process.env.lang) || ''
    },
    async fetchPosts() {
      return this.$content(this.type)
        .sortBy(this.sortBy.key, this.sortBy.direction)
        .limit(this.amount)
        .fetch()
        .catch((err) => {
          error({ statusCode: 404, message: 'Erro ao tentar buscar :c' })
        })
    },
    getTagClass(tag) {
      const slug = tag.replace(' ', '-')
      if (slug.startsWith('increazy-')) {
        return `bg-${slug.replace('increazy-', '')}`
      }

      const avaliable = ['yellow', 'purple', 'orange', 'pink']

      return `bg-${avaliable[Math.floor(Math.random() * avaliable.length)]}`
    },
  },
}
</script>

<style scoped>
.news-container {
  list-style: none;
  margin: 0px;
  padding: 0px;
  justify-content: flex-start;
  align-items: flex-end;
  height: max-content;
}

.new {
  padding: 5px 10px;
  width: 100%;
  height: 160px;
  min-height: 160px;
  max-height: 160px;
}

.new:not(:last-child) {
  margin-bottom: 1px;
}

.new > .row {
  justify-content: space-between;
  height: 100%;
}

h3 {
  font-weight: 300;
  font-size: 24px;
  margin: 0px;
}

.meta {
  display: flex;
  justify-content: space-between;
  align-items: flex-end;
}

.meta * {
  font-size: 13px;
  align-items: flex-end;
}

.meta .tags {
  margin: 0px;
  display: flex;
  align-items: flex-end;
}

.meta .tags .tag {
  margin: 0px;
  padding: 2px;
}

.meta .tags .tag:not(:last-child) {
  margin-right: 1px;
}

.meta[data-inverted='right'] {
  flex-flow: row-reverse;
}

p {
  margin: 6px 0;
}
</style>
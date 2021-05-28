<template>
  <ul v-if="posts.length > 0" class="grid news-container">
    <li v-for="(post, index) in posts" :key="index" class="new">
      <nuxt-link :to="`${postType}/${post.slug}`" class="row">
        <h3 class="col-12" :data-grid="side">
          {{ post.title }}
        </h3>

        <div class="meta col-12 row" :data-grid="side" :data-inverted="side">
          <span class="col">
            {{ formatDate(post.updatedAt) }}
          </span>
          <div class="tags col">
            <span v-for="tag in post.tags" :key="tag" class="tag">
              {{ tag.trim() }}
            </span>
          </div>
        </div>

        <p class="col-12" :data-grid="side">
          {{ post.description }}
        </p>
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
  },
}
</script>

<style scoped>
.news-container {
  list-style: none;
  margin: 0px;
  padding: 0px;
}

.new {
  padding: 10px 5px;
  wdth: 100%;
}

.new,
.new * {
  transition: 0.6s;
}

.new:hover {
  color: #fafafa;
  background: #03151e;
  transition: 0.6s;
}

.new:hover * {
  color: #fafafa;
  transition: 0.6s;
}

.new:hover .meta .tags .tag {
  background: #fafafa;
  color: #03151e;
  transition: 0.6s;
}
/* 
.new:not(:last-child) {
  border-bottom: 1px dashed #03151e;
} */

h3 {
  font-size: 24px;
  margin: 0px;
  margin-bottom: 2px;
}

.meta * {
  font-size: 13px;
}

.meta .tags {
  margin: 0px 15px;
}

.meta .tags .tag {
  background: #03151e;
  color: #fafafa;
  margin: 0px 5px;
  padding: 2px;
}

.meta[data-inverted='right'] {
  flex-flow: row-reverse;
  justify-content: flex-start;
}

p {
  margin: 0;
  margin-top: 2px;
}
</style>
<template>
  <nav
    :class="`row header fg-dark ${classColor} ${classColor !== '' ? 'fg-light' : ''}`"
    data-grid="center-space-between"
  >
    <div class="col-2">
      <nuxt-link v-if="classColor === ''" class="header__logo" to="/">
        <img src="~/assets/imgs/logo.png" alt="Increazy" class="hidden-small" />
        <img src="~/assets/imgs/logo-icon.png" alt="Increazy" class="hidden-large hidden-medium" />
      </nuxt-link>

      <nuxt-link v-if="classColor !== ''" class="header__logo" to="/">
        <img src="~/assets/imgs/logo-white.png" alt="Increazy" class="hidden-small" />
        <img src="~/assets/imgs/logo-icon-white.png" alt="Increazy" class="hidden-large hidden-medium" />
      </nuxt-link>
    </div>

    <ul class="col-10 list--horizontal" data-grid="right">
      <li @click="toggleMenu()" class="fg-purple--hover">
        <nuxt-link to="/docs">Documentação</nuxt-link>
      </li>

      <li @click="toggleMenu()" class="fg-purple--hover">
        <nuxt-link to="/blog">Blog</nuxt-link>
      </li>
    </ul>
  </nav>
</template>

<script>
export default {
  computed: {
    classColor() {
      return this.$store.state.header._class
    },
  },
  watch: {
    classColor() {
      this.$forceUpdate()
    },
  },
}
</script>

<style scoped>
.header {
  align-items: center;
  padding: 10px;
}

.list--horizontal {
  list-style: none;
}

.list--horizontal li {
  margin: 0px 10px;
}

.header__logo {
  display: flex;
  align-items: flex-end;
}

img {
  height: 35px;
  width: auto;
}
</style>
<template>
  <nav class="row header" data-grid="center0space-between">
    <div class="col-2">
      <nuxt-link class="header__logo" to="/">
        <img src="~/assets/imgs/logo.png" alt="Increazy" class="hidden-small" />
        <img src="~/assets/imgs/logo-icon.png" alt="Increazy" class="hidden-large hidden-medium" />
      </nuxt-link>
    </div>

    <ul class="col-10 list--horizontal" data-grid="right">
      <li @click="toggleMenu()">
        <nuxt-link to="/docs">Documentação</nuxt-link>
      </li>

      <li @click="toggleMenu()">
        <nuxt-link to="/blog">Blog</nuxt-link>
      </li>
    </ul>
  </nav>
</template>

<script>
export default {
  name: 'Header',
}
</script>

<style scoped>
.header {
  align-items: center;
  padding: 10px;
  border-bottom: 1px solid #03151e;
}

.list--horizontal {
  list-style: none;
}

.list--horizontal li {
  margin: 0px 10px;
}
</style>
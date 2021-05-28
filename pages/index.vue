<template>
  <main class="grid">
    <section class="row news">
      <h2 class="col-12">
        <span class="cursor"> ᐅ </span>
        Novas postagens
      </h2>
      <new side="left" type="blog" class="col-12" amount="3" />
    </section>

    <section class="row news">
      <h2 class="col-12" data-grid="right">
        Novas documentações
        <span class="cursor">&nbsp;ᐊ </span>
      </h2>
      <new side="right" type="docs" class="col-12" amount="3" />
    </section>
  </main>
</template>

<style scoped>
main.grid {
  width: 80%;
  margin: 0px 10%;
  padding: 50px;
}

.news {
  padding: 20px 0px;
}

.news:not(:last-child) {
  margin-bottom: 50px;
}

h2 {
  font-size: 30px;
}
</style>
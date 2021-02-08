<template>
  <header>
    <div class="bg-white fixed-md-top shadow">
      <nav class="navbar navbar-expand-lg navbar-light bg-white container">
        <nuxt-link class="navbar-brand pl-4" to="/"
          ><h1 class="titulo-general lead">Luis Albanese</h1></nuxt-link
        >
        <button
          class="navbar-toggler"
          type="button"
          data-toggle="collapse"
          data-target="#navbarNavAltMarkup"
          aria-controls="navbarNavAltMarkup"
          aria-expanded="false"
          aria-label="Toggle navigation"
        >
          <span class="navbar-toggler-icon"></span>
        </button>
        <div class="collapse navbar-collapse" id="navbarNavAltMarkup">
          <div class="navbar-nav ml-auto">
            <nuxt-link class="nav-link text-dark" to="/">Inicio</nuxt-link>
            <nuxt-link class="nav-link text-dark" to="/blog"
              >Artículos</nuxt-link
            >
            <nuxt-link class="nav-link text-dark" to="/contacto"
              >Contacto</nuxt-link
            >
          </div>
        </div>
      </nav>
    </div>
  </header>
</template>
<script>
export default {};
</script>

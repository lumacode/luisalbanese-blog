<template>
  <div class="container shadow-none shadow-md mb-5 min">
    <section class="row px-4 pb-5">
      <div class="col-md-12 pt-5">
        <h3 class="titulo-secciones">Artículos</h3>
        <hr />
      </div>
      <div class="col-md-12">
        <div class="form-filter">
          <select v-model="filtroCategoria" class="form-control mb-2 mt-1">
            <option value="">Todas las categorías</option>
            <option
              v-for="categoria in categorias"
              :key="categoria.id"
              :value="categoria.id"
              >{{ categoria.name }}</option
            >
          </select>
        </div>
      </div>

      <div class="col-md-12 mt-5" v-if="searchCategory == ''">
        <p
          class="mt-5 texto-articulo text-primary text-center border p-3 bg-light"
        >
          Parece que no existen artículos compatibles con el filtro ingresado.
          <br />
          Proximamente estaré generando contenido en esta sección.
        </p>
      </div>

      <div
        class="card col-md-6 border-0 m-0 p-0"
        v-for="(articulo, index) in searchCategory"
        :key="index"
        v-show="(pag - 1) * NUM_RESULTS <= index && pag * NUM_RESULTS > index"
      >
        <article class="card-body">
          <h2 class="card-title titulo-secciones">
            <nuxt-link
              :to="`/blog/${slug(articulo.title)}/${articulo.id}`"
              class="link"
              >{{ articulo.title }}</nuxt-link
            >
          </h2>
          <p class="disabled border-bottom pb-3">
            {{ dateFormat(articulo.ts_create) }} ~ {{ articulo.categoryName }} ~
            Autor: {{ articulo.nameUser }} {{ articulo.lastnameUser }}
          </p>
          <nuxt-link :to="`/blog/${slug(articulo.title)}/${articulo.id}`"
            ><img
              :src="
                `${$config.API_URL}/uploads/images/${articulo.img_uri}`
              "
              class="card-img-top"
              alt="trabajo1"
              v-if="articulo.img_uri"
          /></nuxt-link>
          <p
            class="card-text borde-inferior mt-3"
            v-html="articulo.description.slice(0, 350) + '...'"
          ></p>

          <!-- <div class="d-flex justify-content-center"></div>
                            <a href="#" class="btn btn-outline-secondary" target="_blank">Leer artículo</a>
                        </div>-->
        </article>
      </div>

      <!-- Controles -->
      <div class="col-md-12 d-flex justify-content-center">
        <nav aria-label="Page navigation" class="text-center">
          <ul class="pagination text-center">
            <li>
              <a
                href="#"
                class="btn btn-primary"
                aria-label="Previous"
                v-show="pag != 1"
                @click.prevent="pag -= 1"
              >
                <span aria-hidden="true">Anterior</span>
              </a>
            </li>
            <li>
              <a href="#" class="btn btn-primary mx-2" v-show="pag != 1">
                {{ pag }}
              </a>
            </li>
            <li>
              <a
                href="#"
                class="btn btn-primary"
                aria-label="Next"
                v-show="(pag * NUM_RESULTS) / searchCategory.length < 1"
                @click.prevent="pag += 1"
              >
                <span aria-hidden="true">Siguiente</span>
              </a>
            </li>
          </ul>
        </nav>
      </div>
    </section>
  </div>
</template>
<script>
import axios from "axios";
export default {
  data() {
    return {
      NUM_RESULTS: 6, // Numero de resultados por página
      pag: 1, // Página inicial
      filtroCategoria: "",
      countArticles: 0
    };
  },
  async asyncData({
    isDev,
    route,
    store,
    env,
    params,
    req,
    res,
    redirect,
    error,
    $config: { API_URL }
  }) {
    try {
      const res = await axios.get(
        `${API_URL}/articles/getall`
      );
      const articulos = res.data;

      const resCategorias = await axios.get(
        `${API_URL}/categories/getall`
      );
      const categorias = resCategorias.data;

      return { articulos, categorias };
    } catch (error) {
      console.log(error);
    }
  },
  methods: {
    slug(title) {
      const titleSlug = title
        .replace(/[`~!@#$%^&*()_|+\-=?¿;:'",.<>\{\}\[\]\\\/]+/g, "")
        .replace(/\s+/g, "-")
        .normalize("NFD")
        .replace(/[\u0300-\u036f]/g, "")
        .toLowerCase();
      return titleSlug;
    },
    dateFormat(date) {
      const fecha = new Date(date);
      const options = {
        weekday: "long",
        year: "numeric",
        month: "long",
        day: "numeric"
      };
      const format = fecha.toLocaleDateString("es-ES", options);
      return format;
    }
  },
  computed: {
    searchCategory: function() {
      if (this.filtroCategoria !== "") {
        return this.articulos.filter(
          item => item.category_id == this.filtroCategoria
        );
      }

      return this.articulos;
    }
  },
  head: {
    title: 'Luis Albanese | Blog de desarrollo web profesional',
    meta: [
      { hid: 'description', name: 'description', content: 'Contenido relacionado al desarrollo web. Guías de programación, diseño y maquetación web. Cursos de desarrollo web gratis.' },
      { hid: 'author', name: 'author', content: 'Luis Albanese'}
    ]
   }
};
</script>

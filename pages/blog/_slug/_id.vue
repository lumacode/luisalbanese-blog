<template>
  <div>
    <div class="container">
      <section>
        <div class=" px-4 mt-3">
          <div class="pt-4 text-dark">
            <h3 class="titulo-articulo lead">{{ articulo.title }}</h3>
            <p class="disabled">
              {{ dateFormat(articulo.ts_create) }} ~
              {{ articulo.categoryName }} ~ Autor: {{ articulo.nameUser }}
              {{ articulo.lastnameUser }}
            </p>
          </div>
          <img
            :src="
              `https://posicionatedigital.com.ar/api/uploads/images/${articulo.img_uri}`
            "
            class="card-img-top"
            alt="trabajo1"
            v-if="articulo.img_uri"
          />
          <p class="py-2 border-bottom">
            <!--<small>Fuente de la imágen: {{ articulo.notes }}</small>-->
            <small class="mr-1">Compartir en: </small>
            <a :href="`https://www.facebook.com/sharer/sharer.php?u=luisalbanese.com.ar/blog/${slug(articulo.title)}/${articulo.id}`" class="git" target="_blank"><img src="~/assets/img/facebook.png" alt="facebook" title="Compartir en facebook"></a>
            <a :href="`https://twitter.com/intent/tweet?text=luisalbanese.com.ar/blog/${slug(articulo.title)}/${articulo.id}`" class="git" target="_blank"><img src="~/assets/img/twitter.png" alt="twitter" title="Compartir en twitter"></a>
            <a :href="`whatsapp://send?text=Te%20comparto%20este%20artículo%20muy%20interesante%20https://luisalbanese.com.ar/blog/${slug(articulo.title)}/${articulo.id}`" target="_blank" rel="noopener nofollow" class="git d-md-none"><img src="~/assets/img/whatsapp.png" alt="whatsapp" title="Compartir en whatsapp"></a>
          </p>

          <div class="texto-articulo" v-html="articulo.description"></div>

          <!--<p class="text-white bg-dark text-monospace p-5">&lt;p&gt; class="texto-articulo bg-dark p-5 text-white text-monospace"&gt;&lt;div&gt;Hola mundo&lt;/div&gt;&lt;/p&gt;</p>-->
        </div>

        <p class="text-center texto-mediano pt-2 text-primary">~ o ~</p>
      </section>

      <section class="row px-4" v-if="filterRelacionados">
        <h3 class="titulo-secciones col-md-12 border-bottom pb-3">
          Artículos relacionados
        </h3>
        <div
          class="card col-md-4 border-0 m-0 p-0"
          v-for="relacion in filterRelacionados"
          :key="relacion.id"
        >
          <article class="card-body">
            <h2 class="card-title titulo-secciones">
              <nuxt-link
                :to="`/blog/${slug(relacion.title)}/${relacion.id}`"
                class="link"
                >{{ relacion.title }}</nuxt-link
              >
            </h2>
            <p class="disabled border-bottom pb-3">
              {{ dateFormat(relacion.ts_create) }} ~
              {{ relacion.categoryName }} ~ Autor: {{ relacion.nameUser }}
              {{ relacion.lastnameUser }}
            </p>
            <nuxt-link :to="`/blog/${slug(relacion.title)}/${relacion.id}`"
              ><img
                :src="
                  `https://posicionatedigital.com.ar/api/uploads/images/${relacion.img_uri}`
                "
                class="card-img-top"
                alt="trabajo1"
                v-if="relacion.img_uri"
            /></nuxt-link>
            <div
              class="card-text borde-inferior mt-3"
              v-html="relacion.description.slice(0, 250) + '...'"
            ></div>

            <!-- <div class="d-flex justify-content-center"></div>
                        <a href="#" class="btn btn-outline-secondary" target="_blank">Leer artículo</a>
                    </div>-->
          </article>
        </div>
      </section>
    </div>

    <Newsletter />
  </div>
</template>

<script>
import axios from "axios";
import Newsletter from "../../../components/Newsletter";
export default {
  components: { Newsletter },
  async asyncData({
    isDev,
    route,
    store,
    env,
    params,
    req,
    res,
    redirect,
    error
  }) {
    try {
      const res = await axios.get(
        `https://posicionatedigital.com.ar/api/articles/getone/${params.id}`
      );

      const articulo = res.data;

      const resRelacionados = await axios.get(
        `https://posicionatedigital.com.ar/api/articles/relatedposts/${articulo.category_id}`
      );
      const relacionados = resRelacionados.data;

      const filterRelacionados = relacionados.filter(
        relacion => relacion.id !== params.id
      );

      return { articulo, filterRelacionados };
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
  head: {
    title: 'Luis Albanese | blog de desarrollo web profesional',
    meta: [
      { hid: 'description', name: 'description', content: 'Contenido relacionado al desarrollo web. Guías de programación, diseño y maquetación web. Cursos de desarrollo web.' },
      { hid: 'author', name: 'author', content: 'Luis Albanese'}
    ]
   }
};
</script>

<template>
  <div>
    <Banner />
    <div class="container">
      <section>
        <div class="bg-white p-4">
          <h3 class="text-dark titulo-secciones">Sobre mí</h3>
          <hr />

          <p class="pt-3 text-18">
            Soy <strong>desarrollador web full stack hace más de 6 años,</strong> me apasiona construir
            aplicaciones web de todo tipo. A pesar de manejar tanto el campo del front como del back, mi especialización el del lado del servidor. 
          </p>

          <p class="text-18">
            Entre las tecnologías que manejo se encuentran <strong>HTML, CSS, PHP y JavaScript (vue, nuxt, react, node.js)</strong>. <br> 
            La arquitectura que suelo usar para desarrollar es la de tipo API REST, manteniendo, en lo posible, backend, datos y frontend separados. <br>
            Mi framework de front javaScript favorito es <strong>Vue.js</strong> y para el SSR <strong>Nuxt...</strong><br class="d-md-none">
            <small v-if="!displaySobreMi"><a href="#" @click.prevent="mostrarSobreMi" class="text-primary border p-1 ml-md-2">Ver más</a></small>
          </p>

            <div v-if="displaySobreMi">

                <p class="text-18">
                  Me costo mucho tomar el valor para dedicarme al desarrollo web de
                  forma profesional porque pensaba que no podía con el desafio. Si
                  bien siempre falta mucho por aprender no resigno esfuerzos para
                  ello, me considero comprometido y muy curioso a la hora de conocer
                  nuevas tecnologías.
                </p>

                <p class="text-18">
                  Mi lema es que un desarrollador web no se destaca por sus títulos y
                  certificados, se destaca por su trabajo. 
                </p>

                <p class="text-18">
                  <strong>Una recomendación:</strong> nunca dejes que tus miedos te impidan cumplir tus sueños.
                </p>

                <p class="text-18">
                  Espero poder aportar algo positivo a tu desarrollo profesional y
                  como persona. Gracias por visitar mi blog!<br class="d-md-none">
                  <small v-if="displaySobreMi"><a href="#" @click.prevent="ocultarSobreMi" class="text-primary border p-1 ml-md-2">Ver menos</a></small>
                </p>
            </div>
          <p class="text-18 aler alert-success p-4 mt-5 mb-4">
            <b>📢 Aviso:</b> Estoy preparando nuevo contenido para vos, no dejes de visitar mi blog y mis redes sociales. ⬇
          </p>
        </div>
      </section>

      <section class="row px-4 pb-5">
        <div class="col-md-12">
          <h3 class="titulo-secciones">Últimos artículos</h3>
          <hr />
        </div>
        <div
          class="card col-md-6 border-0 m-0 p-0"
          v-for="articulo in articulos"
          :key="articulo.id"
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
              {{ dateFormat(articulo.ts_create) }} ~
              {{ articulo.categoryName }} ~ Autor: {{ articulo.nameUser }}
              {{ articulo.lastnameUser }}
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
            <div
              class="card-text borde-inferior mt-3"
              v-html="articulo.description.slice(0, 350) + '...'"
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
import Banner from "../components/Banner.vue";
import Newsletter from "../components/Newsletter";
export default {
  components: { Newsletter, Banner },
  data(){
    return{
      displaySobreMi: false
    }
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
        `${API_URL}?c=articles&a=getlimit&id=6`
      );

      const articulos = res.data;

      return { articulos };
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
    },
    mostrarSobreMi(){
      this.displaySobreMi = true
    },
    ocultarSobreMi(){
      this.displaySobreMi = false
    }

  },
   head: {
    title: 'Luis Albanese | Desarrollo web profesional',
    meta: [
      { hid: 'description', name: 'description', content: 'Contenido relacionado al desarrollo web. Guías de programación, diseño y maquetación web. Cursos de desarrollo web.' },
      { hid: 'author', name: 'author', content: 'Luis Albanese'}
    ]
   }
};
</script>

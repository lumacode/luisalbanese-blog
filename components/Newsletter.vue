<template>
<section class="bg-light pb-4 pt-1 shadow">
      <div class="p-0 m-0 mt-5">
        <p class="text-center lead"><strong>Suscribite para obtener novedades sobre desarrollo web</strong></p>
                              <transition name="fade">
                      <div class="d-flex justify-content-center" v-if="mensaje !== ''">

                          <span :class="checkNews">{{ mensaje }}</span>

                      </div>
                      </transition>
                  <form @submit.prevent="suscribirse">
                      <div class="d-flex justify-content-center">

                              <div class="row">

                                  <div class="mb-3 p-0 col-md-12">
                                      <input name="email" type="email" class="form-control" placeholder="Ingresa tu email" required v-model="email">
                                  </div>
                                  <div class="mb-4 p-0 col-md-12">
                                      <input name="submit" type="submit" class="btn btn-primary btn-block" value="Suscribirse">
                                  </div>
                              </div>
                      </div>
                      <!-- end form element -->
                </form>
                <div class="d-flex justify-content-center">
                            <a
                            href="https://www.instagram.com/luisalbanese.web/"
                            target="_blank"
                            title="Seguime en instagram"
                            ><img
                              src="~/assets/img/instagram.png"
                              alt="instagram"
                              class="img-fluid git mr-1"
                          /></a>
                            <a
                            href="https://twitter.com/LuisAlbanese3?s=09"
                            target="_blank"
                            title="Seguime en twitter"
                            ><img
                              src="~/assets/img/twitter.png"
                              alt="twitter"
                              class="img-fluid git mr-1"
                          /></a>
                          <a
                          href="https://github.com/lumacode"
                          target="_blank"
                          title="Seguime en github"
                          ><img
                            src="~/assets/img/git.png"
                            alt="github"
                            class="img-fluid git mr-1"
                        /></a>
                  </div>
        </div>
</section>
</template>

<script>
import axios from 'axios'
export default {
  data(){
    return{
      email : '',
      mensaje: '',
      statusNews: false
    }
  },
  methods:{
   async suscribirse(){
     const config = {
                method: 'post',
                url: 'https://posicionatedigital.com.ar/api/contact/newsletter',
                data :  JSON.stringify({"email": this.email})
          }
      const res = await axios(config);
      if(res.data.status == "ERROR"){
        this.mensaje = res.data.message
      }
      if(res.data.status == "OK"){
        this.mensaje = res.data.message
        this.statusNews = true
      }

      this.email = ''
    }
  },
  computed:{
    checkNews(){
      return this.statusNews == true ? 'alert alert-success' : 'alert alert-danger'
    }
  }
}
</script>


<template>
<div>
<div class="container shadow-none shadow-md mb-5">
         <section class="row px-4 pb-4">
            <div class="col-md-12 pt-5"><h3 class="titulo-secciones">Contacto</h3> <hr></div>


                <div class="col-md-12">
                    <div class="mb-2 px-3 mt-3 border alert alert-info">
                        <p class="mt-3 lead info-contacto">Gracias por visitar mi sitio web. A continuación encontrarás la información para ponerte en contacto:</p>
                        <p class="lead info-contacto">Email: info@luisalbanese.com.ar</p>

                    </div>
                </div>


        </section>
    <section class="px-4 py-1">
            <h3 class="titulo-secciones">Formulario de contacto</h3>
            <p>Por favor completa el formulario y luego da click en "enviar mensaje" para que pueda ponerme en contacto a la brevedad.</p>
            <transition name="fade">
                      <div class="d-flex justify-content-center text-center" v-if="resMensaje !== ''">

                          <span :class="checkContact">{{ resMensaje }}</span>

                      </div>
                      </transition>
            <form @submit.prevent="enviar">
                <div class="row">

                    <div class="col-md-6 form-group">
                        <input type="text" class="form-control" placeholder="Ingrese su nombre"  required v-model="nombre">
                    </div>
                    <div class="col-md-6 form-group">
                        <input type="email" class="form-control" placeholder="Ingrese su e-mail"  required v-model="email">
                    </div>
                    <div class="col-md-6 form-group">
                        <input type="text" class="form-control" placeholder="Ingrese su teléfono"  required v-model="telefono">
                    </div>
                    <div class="col-md-6 form-group">
                        <input type="text" class="form-control" placeholder="Ingrese un asunto"  required v-model="asunto">
                    </div>
                    <div class="col-12 form-group">
                        <textarea class="form-control" rows="3" placeholder="Ingrese mensaje"  required v-model="mensaje"></textarea>
                    </div>
                    <div class="col-12 mb-4">
                        <input type="submit" class="btn btn-success btn-block" value="Enviar mensaje">
                    </div>
                </div>
                <!-- end form element -->
            </form>

        </section>

   </div>
<Newsletter />
</div>
</template>
<script>
import axios from 'axios'
export default {
  data(){
    return{
      nombre: '',
      email: '',
      telefono: '',
      asunto: '',
      mensaje: '',
      statusContact: false,
      resMensaje: ''
    }
  },
  methods:{
     async enviar(){

       const data = {
                    "subject": this.asunto,
                    "body":{
                            "name" : this.nombre,
                            "email": this.email,
                            "phone" : this.telefono,
                            "message" : this.mensaje
                            }
                    }
      const config = {
                  method: 'post',
                  url: 'https://api.posicionatedigital.com.ar?c=contact&a=contactform',
                  data :  JSON.stringify(data)
            }

      const res = await axios(config);
      if(res.data.status == "ERROR"){
        this.resMensaje = res.data.message

      }
      if(res.data.status == "OK"){
        this.resMensaje = res.data.message
        this.statusContact = true
        //Se borran los campos
        this.email = ''
        this.nombre = ''
        this.telefono = ''
        this.mensaje = ''
        this.asunto = ''
      }


    }
  },
  computed:{
    checkContact(){
      return this.statusContact === true ? 'alert alert-success w-100' : 'alert alert-danger w-100'
    }
  },
  head: {
    title: 'Luis Albanese | Cursos de desarrollo web',
    meta: [
      { hid: 'description', name: 'description', content: 'Aprender desarrollo web gratis. Cursos de HTML, CSS, PHP, javascript, Node, Vue, React.' },
      { hid: 'author', name: 'author', content: 'Luis Albanese'}
    ]
   }

}
</script>

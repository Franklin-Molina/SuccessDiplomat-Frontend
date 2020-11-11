<template>
    <div>
           <div class="container d-flex justify-content-center mb-4">
        
<li href="#" class="btnhome2">

 <router-link to="/myproduct" id="col_border"
          ><h1 class="text-align-center">Volver</h1></router-link
        >
    
  </li>
        </div>
        <h1>Editando el Producto</h1>

          @change="prevista"
         <form
          @submit.prevent="guardar"
          
          enctype="multipart/form-data"
        >
        
   

          <label for="nombre">Nombre</label> <br>
          <input type="text" name="nombre" v-model="nombre"><br>
          <label for="descripcion">Descripcion</label><br>
          <input type="text" name="descripcion" v-model="descripcion"><br>
          <label for="celular">Celular</label><br>
          <input type="text" name="celular" v-model="celular"><br>

            <label for="">Actualizar Logo</label>
  <input type="file" ref="file" id="file" accept="image/*"  /> <br> <br>

     <div v-if="loading">
              <div class="mt-2">
                <b-alert
                  :show="dismissCountDown"
                  dismissible
                  variant="success"
                  @dismissed="dismissCountDown = 0"
                  @dismiss-count-down="countDownChanged"
                >
                  <p><strong>Success !</strong> Producto Actualizado Correctamente</p>
                  <b-progress
                    variant="success"
                    :max="dismissSecs"
                    :value="dismissCountDown"
                    height="4px"
                  ></b-progress>
                </b-alert>
              </div>
            </div>
            
    <div class="row">
        <div
          class="col-12 col-sm-12 col-md-6 col-lg-4"
          v-for="items in galeria"
          :key="items.id"
        >
          <Misproductos :items="items" />

        </div>
      </div>


     <b-button type="submit" @click="showAlert"  id="color" block variant="dark"  onsubmit="setTimeout(function () { window.location.reload(); }, 10)"  >
              Guardar
    </b-button>
       
   </form>


    </div>
</template>


<script>

import Misproductos from "@/components/Misproductos.vue";
import axios from 'axios'

export default {
    name : "EditarProduct",
     components: {
    Misproductos,
  },
        data() {
            return {
                nombre: "",
                descripcion: "",
                celular: "",
                loading: false,
                 dismissSecs: 5,
                 dismissCountDown: 0,
                 galeria: [],
            }
        },

        mounted (){
           this.misimagenes();
          
            
                    this.token = localStorage.getItem('token');
                    this.id = this.$route.params.id

                    axios.get("http://localhost:1337/productos/" + this.id,{
                            
                        headers: {
                            'Authorization': 'Bearer ' + this.token,
                        
                    },

                    }).then((response) => {
                       
                        this.nombre = response.data.nombre
                        this.descripcion = response.data.descripcion
                         this.celular = response.data.celular
                    })
                            
                        },

                methods: {
                            
                         countDownChanged(dismissCountDown) {
                        this.dismissCountDown = dismissCountDown;
                        },
                        showAlert() {
                        this.dismissCountDown = this.dismissSecs;
                        },

                    guardar(){                  
                    this.loading = false;

                        axios.put("http://localhost:1337/productos/"+ this.id,{

                            nombre: this.nombre,
                            descripcion: this.descripcion,
                            celular: this.celular,
                          
                        },
                        {
                             headers: {
                             'Authorization': 'Bearer ' + this.token
                          },

                            }).then((response) => {
                                this.loading = true
                            //   this.$router.push('/myproduct')
                            })                        
                   },
                misimagenes() {
                this.user = JSON.parse(localStorage.getItem("user"));
                //recuperar el token para la Auth
                //const tokenverificacion =
                fetch("http://localhost:1337/productos/me", {
                    headers: {
                    Authorization: "Bearer " + localStorage.getItem("token"),
                    },
                })
                    .then((res) => res.json())
                    .then((data) => {
                    console.log(data);
                    this.galeria = data;
                    });
                },
                
                    
 },


}
</script>
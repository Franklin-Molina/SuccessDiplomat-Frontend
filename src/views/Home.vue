<template>
  <div>
    <b-navbar toggleable="md" type="dark" class="Nav">
      <b-navbar-brand href="/">Inicio</b-navbar-brand>

      <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>

      <b-collapse id="nav-collapse" is-nav>
        <!-- <b-navbar-nav>
        
        <b-nav-item href="/registro">Registro</b-nav-item>
         <b-nav-item href="/home" >Categorias</b-nav-item>
      </b-navbar-nav> -->

        <!-- Right aligned nav items -->
        <b-navbar-nav class="ml-auto">
          <b-nav-item-dropdown right>
            <!-- Using 'button-content' slot -->
            <template #button-content>
              <em>Mi Perfil</em>
            </template>

            <b-dropdown-item @click="salir">Sign Out</b-dropdown-item>
          </b-nav-item-dropdown>
        </b-navbar-nav>
      </b-collapse>
    </b-navbar>

    <div class="container-flex">
      <b-carousel
        id="carousel-1"
      
      
        controls
        indicators
        background="#ababab"
        img-width="1024"
        img-height="480"
        style="text-shadow: 1px 1px 2px #333"
       
      >
        <!-- Text slides with image -->

        <!-- Slides with custom text -->
        <b-carousel-slide
          img-src="https://picsum.photos/1024/480/?image=14"
          style="max-height: 400px"
        >
          <p class="letra">
            Bienvenido! {{ user.username }}
            <!-- Correo: {{ user.email }} -->
          </p>

          <div>
           
            <h1>Mis Imagenes</h1>
          </div>
        </b-carousel-slide>
      </b-carousel>
    </div>

    <div class="container mt-4">
      <div class="row">
        <div class="col-12"></div>
      </div>

     
<div v-if="complete" class="container d-flex justify-content-center stydis"> 
  <li href="#" class="btnhome">


 <router-link to="/cargarimagen" id="col_border"><h1 class="text-align-center disable "  disabled>SUBIR IMAGENES</h1></router-link
        >
    
  </li>

</div>

 <br> <br>
      
      <div class="row">
        <div
          class="col-12 col-sm-12 col-md-6 col-lg-4"
          v-for="items in galeria"
          :key="items.id"
        >
          <Galeria :items="items" />

          <b-button class="tam" variant="danger" @click="eliminar(items.id)"
            >Eliminar</b-button
          >
        </div>
      </div>
    </div>
    <div>
      <h1> Registro empresa</h1>

       <form @submit.prevent="register"  enctype="multipart/form-data">

         <label for="nombre">Nombre social de la empresa</label> <br>
        <input type="text" v-model="nombre"> <br>

          <input type="file" ref="file" id="file" accept="image/*" required />

        <button type="submit">Enviar </button>


      </form>




       <div class="ingre mt-2 mb-2">
                  <a id="col_border" href="/product">
                    <button
                      type="button"
                      id="color"
                      class="btn btn-dark btn-lg btn-block"
                    >
                     Crear Productos
                    </button>
                  </a>
      </div>

    </div>




  </div>



</template>

<script>
// @ is an alias to /src
import Galeria from "@/components/Galeria.vue";
import axios from "axios";

export default {
  name: "Home",
  components: {
    Galeria,
  },

  data() {
    return {
      galeria: [],
      user: {},
      tags: [],
      selectedFile: null,
      complete: false,
      nombre: "",
    };
  },

  mounted() {
    this.misimagenes();
 
  },

  methods: {
    salir() {
      localStorage.removeItem("token");
      localStorage.removeItem("user");
      this.$router.push("/");
    },
  
   

    misimagenes() {
      this.user = JSON.parse(localStorage.getItem("user"));
      //recuperar el token para la Auth
      //const tokenverificacion =
      fetch("http://localhost:1337/imagenes/me/", {
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
  /*  mistags(){
      this.user = JSON.parse(localStorage.getItem("user"));
      //recuperar el token para la Auth
      //const tokenverificacion =
      fetch("http://localhost:1337/tags/me/", {
        headers: {
          Authorization: "Bearer " + localStorage.getItem("token"),
        },
      })
        .then((res) => res.json())
        .then((data) => {
          console.log(data);
          this.tags = data;
        });

    },*/

    eliminar(id) {
      axios
        .delete("http://localhost:1337/imagenes/" + id, {
          headers: {
            Authorization: "Bearer " + localStorage.getItem("token"),
          },
        })
        .then((response) => {
          this.misimagenes();
        });
    },

    register(){

      const tokenverificacion = localStorage.getItem('token')
            axios.post("http://localhost:1337/imagenes",{
                nombre: this.nombre,
            },
            {
              headers: {
            'Authorization': 'Bearer ' + tokenverificacion
         
         },

            })
            
            .then((response) => {
                //this.$router.push('/tag')
            })
    }
  },
};
</script>
<style >
.tam {
  width: 300px;
}
.letra {
  font: oblique bold 120% cursive;
  font-size: 56px;
}
.btnhome {
    font-family: Lato, sans-serif;
  text-decoration: none;
width: 1000px;
  border: 5px solid #2c3e50;
  color: #2c3e50;
  display: block;
 
  font-size: 2rem;
  letter-spacing: 0.1rem;
  padding: 1rem;
  position: relative;
  text-decoration: none;
  text-transform: uppercase;
}

.btnhome::before {
  content: "";
  background-color: #E26A6A;
  box-shadow: 10px 10px 0 #F1C40F, 
              20px 20px 0 #3498DB;
  position: absolute;
  left: 0.25rem;
  top: 0.5rem;
  height: 102%;
  width: 102%;
  z-index: -1;
  transition: all 0.4s ease;
}

.btnhome:hover::before {
  box-shadow: none;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
}


</style>
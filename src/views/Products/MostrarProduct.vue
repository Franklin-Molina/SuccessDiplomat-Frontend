<template>
  <div>
    

    

    <div class="container mt-4">
          <div class="container d-flex justify-content-center mb-4">
        
<li href="#" class="btnhome2">

 <router-link to="/" id="col_border"
          ><h1 class="text-align-center">Volver</h1></router-link
        >
    
  </li>
        </div>
      
     <p>Productos</p>
         
      <div class="row">
        <div
          class="col-12 col-sm-12 col-md-6 col-lg-4"
          v-for="items in galeria"
          :key="items.id"
        >
          <Misproductos :items="items" />


        
          
        </div>
      </div>
      
    </div>
    <div>
    </div>


<p> </p>

  </div>



</template>

<script>
// @ is an alias to /src
import Misproductos from "@/components/Misproductos.vue";
import axios from "axios";

export default {
  name: "Home",
  components: {
    Misproductos,
  },

  data() {
    return {
      galeria: [],
      user: {},
      tags: [],
      selectedFile: null,
      complete: true,
      nombre: "",
    };
  },

  mounted() {
    this.misimagenes();
 
  },

  methods: {
   
   

    misimagenes() {
  
      //recuperar el token para la Auth
      //const tokenverificacion =
      fetch("http://localhost:1337/productos", {
     
      })
        .then((res) => res.json())
        .then((data) => {
          console.log(data);
          this.galeria = data;
        });
    },
 

   

   
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
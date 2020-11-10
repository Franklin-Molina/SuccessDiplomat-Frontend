<template>
  <div class="row p-2 bg-white sticky-top mx-0">
    <div class="col-12 align-self-center text-center">

        
        <div class="container d-flex justify-content-center mb-4">
        
<li href="#" class="btnhome2">

 <router-link to="/home" id="col_border"
          ><h1 class="text-align-center">Volver</h1></router-link
        >
    
  </li>
        </div>
         
      <h1>Crear Imagen</h1>
    </div>
    <hr />
    <div class="row mx-0">
      <div class="col-12 col-md-6 text-star mx-0">
       
        <form
          @submit.prevent="subirimagen"
          @change="prevista"
          enctype="multipart/form-data"
        >
          <input type="file" ref="file" id="file" accept="image/*" required /> <br><br>

         <label for="nombre">Nombre Social</label> <br>
          <input type="text" name="nombre" v-model="nombre"><br>
          <label for="email">Email</label><br>
            <input type="email" name="email" v-model="email">


       <div v-if="loading">
              <div class="mt-2">
                <b-alert
                  :show="dismissCountDown"
                  dismissible
                  variant="success"
                  @dismissed="dismissCountDown = 0"
                  @dismiss-count-down="countDownChanged"
                >
                  <p><strong>Success !</strong> Se Registro Correctamente</p>
                  <b-progress
                    variant="success"
                    :max="dismissSecs"
                    :value="dismissCountDown"
                    height="4px"
                  ></b-progress>
                </b-alert>
              </div>
            </div>
            

          <b-button
          class="mt-2"
            type="submit"
            id="colorid"
            @click="showAlert"
            onsubmit="setTimeout(function () { window.location.reload(); }, 10)"
          >
            <b-icon icon="cloud-upload" aria-hidden="true"></b-icon> Enviar
            
          </b-button>

          
        </form>

         
        
        </div>
      </div>
      <div
        class="col-12 col-md-6 text-center justify-content-center mx-auto"
        style="height: 500px"
      >
        <p><b>Previsualizacion:</b></p>
        <br />
        <div v-if="image === ''"></div>
        <div>
          <img  :src="image" alt=""  height="400px" width="400px"/>
        </div>
      </div>
    </div>
  
</template>

<script>
import axios from "axios";

export default {
  name: "CargaImagen",
  data() {
    return {
      cat: [],
      checkedNames: [],
      image: "",
      subirimage: "",
      alerta: false,
      dismissSecs: 5,
      dismissCountDown: 0,
      nombre: '',
      email: "",
       variants: [ 'success'],
       loading: false
    };
  },
  mounted() {
    axios.get("http://localhost:1337/tags/me",{

         headers: {
            Authorization: "Bearer " + localStorage.getItem("token"),
          },
    }).then((response) => {
      
      this.cat = response.data;
    });
  },
  methods: {
    countDownChanged(dismissCountDown) {
      this.dismissCountDown = dismissCountDown;
    },
    showAlert() {
      this.dismissCountDown = this.dismissSecs;
    },

    prevista(e) {
      this.image = URL.createObjectURL(e.target.files[0]);
      this.subirimage = e.target.files[0];
    },
    subirimagen() {

      this.loading = false
       
        this.alerta = false;
        const token = localStorage.getItem("token");
        var formData = new FormData();
        let data = { tags: this.checkedNames };
        formData.append("files.imagen", this.subirimage);
        //formData.append("data", JSON.stringify(data));
        formData.append("data", JSON.stringify({
            nombre: this.nombre,
            email: this.email,
            

        }));

        console.log(this.subirimage);

        axios
          .post("http://localhost:1337/imagenes", formData, {

            nombre: this.nombre,
             headers: {
              Authorization: "Bearer " + token,
              "Content-Type": "multipart/form-data",
              //  "Content-Type": "application/json",
            },
          },
               
         
           
         )
          .then((response) => {
            this.loading= true
        //    this.$router.push("/home");
          })
           .catch((err) => {
          console.log("Fallo");
          this.error = true;
        });
          
      
    },
  },
};
</script>

<style >
.colorsub {
  background-color: green;
}

#colorid {
  background: green;
}



/* https://upload.wikimedia.org/wikipedia/commons/thumb/2/27/White_check.svg/1200px-White_check.svg.png 
*/

.custom-checkbox {
  width: 35px;
  height: 35px;
  cursor: pointer;
}

.custom-checkbox .checkmark {
  width: 100%;
  height: 100%;
  border: 2px solid #222;
  display: inline-block;
  border-radius: 3px;
  background: #222 url(https://upload.wikimedia.org/wikipedia/commons/thumb/2/27/White_check.svg/1200px-White_check.svg.png) center/1250% no-repeat;
  transition: background-size 0.2s ease;
}

.custom-checkbox input:checked + .checkmark {
  background-size: 60%;
  transition: background-size 0.25s cubic-bezier(0.7, 0, 0.18, 1.24);
}

.custom-checkbox input {
  display: none;
}

.btnhome2 {
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

.btnhome2::before {
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

.btnhome2:hover::before {
  box-shadow: none;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
}
</style>
<template>
    <div>
        <h1>Productos</h1>


    <input type="file" ref="file">

    <button  type="button " @click="uploadimage"> Enviar</button>


    </div>
</template>

<script>
export default {

    name: "Product",

    data() {
        return {
              imagen: null
        }

       
    },
    
     methods: {

            abriarchivo(){
                this.$refs.file.click()
            },
            cambiararchivo(e){
                this.imagen = e.target.files[0]
            },
            uploadimage(){

                console.log(this.imagen)
            }
        },
}
</script>
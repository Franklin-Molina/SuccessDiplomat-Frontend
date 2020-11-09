<template>

<div>
  <h1>Listado de Tags</h1>
  <a href="/tag/create">Crear </a> <br />

 
  <table>
    <thead>
      <tr>
        <td>ID</td>
        <td>Nombre</td>
        <td>Acciones</td>
      
      </tr>
    </thead>
    <tbody>
      <tr v-for="tag in tags" :key="tag.id">
        <td>{{ tag.id }}</td>
        <td>{{ tag.nombre }}</td>
        <td>
          <a :href="`/tags/${tag.id}/edit`">Editar</a>
          <a href="#" @click="eliminar(tag.id)">Eliminar</a>
        </td>
      </tr>
    </tbody>
  </table>

</div>
  
</template>
<script>
import axios from "axios";
import debounce from "debounce";
export default {
  name: "Tags",
  data() {
    return {
      tags: [],
      
    };
  },

  mounted() {
    
    this.traerTags();
  },
  /*Para buscar*/
 
  methods: {
    traerTags() {
      axios
        .get("http://localhost:1337/tags/me", {
            headers: {
            Authorization: "Bearer " + localStorage.getItem("token"),
          },
        })
        .then((response) => {
          this.tags = response.data;
        });
    },

    eliminar(id) {
      axios
        .delete("http://localhost:1337/tags/" + id, {
          headers: {
            Authorization: "Bearer " + localStorage.getItem("token"),
          },
        })
        .then((response) => {
          this.traerTags();
        });
    },
   
  },
};
</script>
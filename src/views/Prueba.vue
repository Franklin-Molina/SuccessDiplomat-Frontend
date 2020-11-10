<template>
  <div>
      
    <div class="text-center mb-3 d-flex justify-content-between">
      <b-spinner
        v-for="variant in variants"
        :variant="variant"
        :key="variant"
      ></b-spinner>
    </div>

  
  </div>
</template>

<script>
  export default {
    data() {
      return {
        variants: [ 'success']
      }
    }
  }
</script>
<template>
<div class="container">
    
</div>
</template>
<script>
import moment from 'moment-timezone';
import Skycon from 'vue-skycons';
export default {
  name: 'Forecast',
  components:{
    Skycon
  },
  props:{
    weather:Object,
    location:Object
  },
  data(){
    return{

    }
  }
}
</script>
<style scoped>
*{
    font-family: system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
    color:white
}
.container{
    background-color:rgb(50, 46, 46);
    color:rgb(178, 202, 89);
    padding:1rem;
    padding-top:0;
    border:1px solid black;
    border-radius:1rem
}
</style>
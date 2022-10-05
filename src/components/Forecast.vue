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
</style>
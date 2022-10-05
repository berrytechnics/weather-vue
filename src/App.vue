<template>
  <!-- <Forecast /> -->
  <Current />
</template>
<script>
  import Current from './components/Current'
  import Forecast from './components/Forecast'
export default {
  name: 'Weather',
  components:{
    Current,
    Forecast
  }
}
  
</script>
<style scoped>

</style>
<template>
<div class="container">
    <h1>test</h1>
</div>
</template>
<script>
import axios from 'axios';
import moment from 'moment-timezone';
import Skycon from 'vue-skycons';
import geoFind from 'reverse-geocode';
export default {
  name: 'Forecast',
  components:{
    Skycon
  },
  data(){
    return{
      location:{},
      weather:{
        current:{}
      },
      // Starts on Sunday
      day:[] 
    }
  },
  methods:{
    getF:(K)=>Math.round(((K-273.15)*1.8)+32),
    getHg:(P)=>Math.round(P*0.029529983071445),
    getDateTime:(T)=>moment(new Date(T*1000)).format('MM/DD/YYYY h:mm A'),
    getTime:(T)=>moment(new Date(T*1000)).format('h:MM A'),
    getLoc:(lat,lon)=>geoFind.lookup(lat,lon,'us')
  },
  mounted(){
    navigator.geolocation.getCurrentPosition(async(pos)=>{
      const response = await axios.get(`https://api.openweathermap.org/data/3.0/onecall?lat=${pos.coords.latitude}&lon=${pos.coords.longitude}&appid=${process.env.VUE_APP_API_KEY}`)
      this.weather = response.data
      this.weather.current.dt = this.getDateTime(this.weather.current.dt)
      this.weather.current.sunrise = this.getTime(this.weather.current.sunrise)
      this.weather.current.sunset = this.getTime(this.weather.current.sunset)
      this.weather.current.temp = this.getF(this.weather.current.temp)
      this.weather.current.feels_like = this.getF(this.weather.current.feels_like)
      this.weather.current.dew_point = this.getF(this.weather.current.dew_point)
      this.weather.current.pressure = this.getHg(this.weather.current.pressure)
      this.location = this.getLoc(pos.coords.latitude,pos.coords.longitude)
    },(err)=>alert('Your browser does not support this app!'))
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
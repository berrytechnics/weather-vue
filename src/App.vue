<template>
  <div class="container">
    <div class="header">
      <h1>Weather for {{location.city}}</h1>
    </div>
    <div class="body">
      <ul>
      <li>Sunrise: {{weather.current.sunrise}}</li>
      <li>Sunset: {{weather.current.sunset}}</li>
      <li>Temp: {{weather.current.temp}}&deg;F</li>
      <li>Feels like: {{weather.current.feels_like}}&deg;F</li>
      <li>Humidity: {{weather.current.humidity}}%</li>
      <li>Dew Point: {{weather.current.dew_point}}&deg;F</li>
      <li>Pressure: {{weather.current.pressure}} mm/Hg</li>
    </ul>
    <br />
    <div id="canvas-container">
      <Skycon v-if="icon=='Thunderstorm'||icon=='Drizzle'||icon=='Rain'" condition="rain" size="400"/>
      <Skycon v-if="icon=='Snow'" condition="snow" size="400"/>
      <Skycon v-if="icon=='Atmosphere'" condition="fog" size="400"/>
      <Skycon v-if="icon=='Clouds'" condition="cloudy" size="400"/>
      <Skycon v-if="icon=='clear-night'" condition="clear-night" size="400"/>
      <Skycon v-if="icon=='clear-day'" condition="clear-day" size="400"/>
    </div>
  </div>
  </div>
</template>
<script>
import axios from 'axios';
import moment from 'moment-timezone';
import Skycon from 'vue-skycons';
import geoFind from 'reverse-geocode';
export default {
  name: 'Weather',
  components:{
    Skycon
  },
  data(){
    return{
      lat:null,
      lon:null,
      sundown:null,
      location:{},
      weather:{
        current:{}
      },
      icon:""
    }
  },
  mounted(){
    if((!"geolocation" in navigator)){
      this.lat = 40.6892;
      this.lon = 74.0445
    }
    else{
      navigator.geolocation.getCurrentPosition(async(pos)=>{
      this.lat = pos.coords.latitude
      this.lon = pos.coords.longitude
      const response = await axios.get(`https://api.openweathermap.org/data/3.0/onecall?lat=${this.lat}&lon=${this.lon}&appid=${process.env.VUE_APP_API_KEY}`)
      this.weather = response.data
      this.icon = this.weather.current.weather[0].main
      if(this.icon == "Clear"){
        Date.now()>=this.weather.current.sunset ?  this.icon = "clear-night" : this.icon = "clear-day"
      }
      this.weather.current.dt = moment.unix(this.weather.current.dt)
      this.weather.current.sunrise = Date.now()>this.weather.current.sunrise ? moment.unix(this.weather.current.sunrise).toNow() : moment.unix(this.weather.current.sunrise).fromNow()
      this.weather.current.sunset = Date.now()<this.weather.current.sunset ? moment.unix(this.weather.current.sunset).toNow() : moment.unix(this.weather.current.sunset).fromNow()
      this.weather.current.temp = Math.round(((this.weather.current.temp-273.15)*1.8)+32)
      this.weather.current.feels_like = Math.round(((this.weather.current.feels_like-273.15)*1.8)+32)
      this.weather.current.dew_point = Math.round(((this.weather.current.dew_point-273.15)*1.8)+32)
      // this.weather.hourly.map(i=>{
      //   i.temp = Math.round(((i.temp-273.15)*1.8)+32)
      //   i.feels_like = Math.round(((i.feels_like-273.15)*1.8)+32)
      //   i.dew_point = Math.round(((i.dew_point-273.15)*1.8)+32)
      // })
      this.location = geoFind.lookup(this.lat,this.lon,'us')
    },(err)=>console.log(err))
    }
  }
}
</script>
<style scoped>
*{
  font-family:roboto;
}
.container{
  background-color:rgb(50, 46, 46);
  color:rgb(178, 202, 89);
  padding:1rem;
  padding-top:0;
  border:1px solid black;
  border-radius:1rem
}
h1{
  text-align: center;
}
ul{
  list-style-type:none;
}
li{
  margin:.5rem
}
#canvas-container {
   width: 100%;
   text-align:center;
}

canvas {
   display: inline;
}
</style>

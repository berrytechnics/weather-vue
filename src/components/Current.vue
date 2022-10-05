<template>
    <div class="container current-weather">
      <div class="header">
        <h1>Current weather</h1>
        <h3>for {{location.city}}, {{location.state_abbr}} ({{location.zipcode}})</h3>
      </div>
      <div class="center-container">
        <table>
        <tbody>
          <tr>
            <td class="text-right">Sunrise:</td>
            <td>{{weather.current.sunrise}}</td>
          </tr>
          <tr>
            <td class="text-right">Sunset:</td>
            <td>{{weather.current.sunset}}</td>
          </tr>
          <tr>
            <td class="text-right">Temperature:</td>
            <td>{{weather.current.temp}}&deg;F</td>
          </tr>
          <tr>
            <td class="text-right">Feels Like:</td>
            <td>{{weather.current.feels_like}}&deg;F</td>
          </tr>
          <tr>
            <td class="text-right">Humidity:</td>
            <td>{{weather.current.humidity}}%</td>
          </tr>
          <tr>
            <td class="text-right">Dew Point:</td>
            <td>{{weather.current.dew_point}}&deg;F</td>
          </tr>
          <tr>
            <td class="text-right">Pressure:</td>
            <td>{{weather.current.pressure}}"/Hg</td>
          </tr>
        </tbody>
      </table>
      </div> 
      <div class="center-container">
        <Skycon v-if="icon=='Thunderstorm'||icon=='Drizzle'||icon=='Rain'" condition="rain" size="400"/>
        <Skycon v-if="icon=='Snow'" condition="snow" size="400"/>
        <Skycon v-if="icon=='Atmosphere'" condition="fog" size="400"/>
        <Skycon v-if="icon=='Clouds'" condition="cloudy" size="400"/>
        <Skycon v-if="icon=='clear-night'" condition="clear-night" size="400"/>
        <Skycon v-if="icon=='clear-day'" condition="clear-day" size="400"/>
      </div>
    </div>
</template>
<script>
import axios from 'axios';
import moment from 'moment-timezone';
import Skycon from 'vue-skycons';
import geoFind from 'reverse-geocode';
export default {
  name: 'Current',
  components:{
    Skycon
  },
  data(){
    return{
      location:{},
      weather:{
        current:{}
      },
      icon:""
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
      this.icon = this.weather.current.weather[0].main
      if(this.icon == "Clear"){
        new Date()>=new Date(this.weather.current.sunset*1000) ?  this.icon = "clear-night" : this.icon = "clear-day"
      }
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
  font-family: roboto, Arial, Helvetica, sans-serif;
}
.container{
  background-color:rgb(50, 46, 46);
  color:rgb(178, 202, 89);
  padding:1rem;
  padding-top:0;
  border:1px solid black;
  border-radius:1rem
}
h1,h3{
  text-align: center;
}
.center-container {
    width: 100%;
    text-align:center;
}
canvas,table {
    display: inline;
}
.text-right{
  text-align:right;
  padding-right:5rem
}
</style>
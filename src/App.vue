<template>
  <div class="container">
    <div class="header">
      <h1>Weather for {{location.city}}</h1>
    </div>
    <div class="body">
      <div id="center-container">
        <table>
        <tbody>
          <tr>
            <td class="text-right">Sunrise:</td>
            <td>{{weather.current.sunrise}} AM</td>
          </tr>
          <tr>
            <td class="text-right">Sunset:</td>
            <td>{{weather.current.sunset}} PM</td>
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
    <br />
    <div id="center-container">
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
      location:{},
      weather:{
        current:{}
      },
      icon:""
    }
  },
  methods:{
    getF:(C)=>Math.round(((C-273.15)*1.8)+32),
    getHg:(P)=>Math.round(P*0.296133971008484)
  },
  mounted(){
    navigator.geolocation.getCurrentPosition(async(pos)=>{
      const response = await axios.get(`https://api.openweathermap.org/data/3.0/onecall?lat=${pos.coords.latitude}&lon=${pos.coords.longitude}&appid=${process.env.VUE_APP_API_KEY}`)
      this.weather = response.data
      this.icon = this.weather.current.weather[0].main
      if(this.icon == "Clear"){
        new Date()>=new Date(this.weather.current.sunset*1000) ?  this.icon = "clear-night" : this.icon = "clear-day"
      }
      this.weather.current.dt = moment(new Date(this.weather.current.dt*1000))
      this.weather.current.sunrise = moment(new Date(this.weather.current.sunrise*1000)).format("h:mm")
      this.weather.current.sunset = moment(new Date(this.weather.current.sunset*1000)).format("h:mm")
      this.weather.current.temp = this.getF(this.weather.current.temp)
      this.weather.current.feels_like = this.getF(this.weather.current.feels_like)
      this.weather.current.dew_point = this.getF(this.weather.current.dew_point)
      this.weather.current.pressure = this.getHg(this.weather.current.pressure)
      this.location = geoFind.lookup(pos.coords.latitude,pos.coords.longitude,'us')
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
h1{
  text-align: center;
}
#center-container {
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

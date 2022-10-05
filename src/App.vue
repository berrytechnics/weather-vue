<template>
  <div class="container">
    <div class="header">
      <h1>Weather for {{location.city}}<!-- , {{location.state}} --></h1>
    </div>
    <div class="body">
      <div id="center-container">
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
            <td>{{weather.current.pressure}} mm/Hg</td>
          </tr>
        </tbody>
      </table>
      </div> 
      <!-- <ul>
      <li>Sunrise: {{weather.current.sunrise}}</li>
      <li>Sunset: {{weather.current.sunset}}</li>
      <li>Temp: {{weather.current.temp}}&deg;F</li>
      <li>Feels like: {{weather.current.feels_like}}&deg;F</li>
      <li>Humidity: {{weather.current.humidity}}%</li>
      <li>Dew Point: {{weather.current.dew_point}}&deg;F</li>
      <li>Pressure: {{weather.current.pressure}} mm/Hg</li>
    </ul> -->
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
  mounted(){
    navigator.geolocation.getCurrentPosition(async(pos)=>{
      const response = await axios.get(`https://api.openweathermap.org/data/3.0/onecall?lat=${pos.coords.latitude}&lon=${pos.coords.longitude}&appid=${process.env.VUE_APP_API_KEY}`)
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
      this.location = geoFind.lookup(pos.coords.latitude,pos.coords.longitude,'us')
    },(err)=>alert('Your browser does not support this app!'))
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
/* ul{
  list-style-type:none;
}
li{
  margin:.5rem
} */

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

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
        <Skycon v-if="wxIcon=='clear-day'" condition="clear-day" size="400" />
        <Skycon v-if="wxIcon=='clear-night'" condition="clear-night" size="400" />
        <Skycon v-if="wxIcon=='fog'" condition="fog" size="400" />
        <Skycon v-if="wxIcon=='rain'" condition="rain" size="400" />
        <Skycon v-if="wxIcon=='snow'" condition="snow" size="400" />
        <Skycon v-if="wxIcon=='cloudy'" condition="cloudy" size="400" />
      </div>
    </div>
</template>
<script>
import Skycon from 'vue-skycons';
export default {
  name: 'Current',
  components:{
    Skycon
  },
  props:{
    weather:Object,
    location:Object,
    wxIcon:String
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
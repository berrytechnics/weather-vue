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
            <td>{{weather.sunrise}}</td>
          </tr>
          <tr>
            <td class="text-right">Sunset:</td>
            <td>{{weather.sunset}}</td>
          </tr>
          <tr>
            <td class="text-right">Temperature:</td>
            <td>{{weather.temp}}&deg;F</td>
          </tr>
          <tr>
            <td class="text-right">Feels Like:</td>
            <td>{{weather.feels_like}}&deg;F</td>
          </tr>
          <tr>
            <td class="text-right">Humidity:</td>
            <td>{{weather.humidity}}%</td>
          </tr>
          <tr>
            <td class="text-right">Dew Point:</td>
            <td>{{weather.dew_point}}&deg;F</td>
          </tr>
          <tr>
            <td class="text-right">Pressure:</td>
            <td>{{weather.pressure}}"/Hg</td>
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
</style>
<template>
<div class="container">
  <h1>Forecast</h1>
  <div class="container-flex">
    <div class="item" v-for="day in weather">
      <table>
        <thead>
          <tr>
            <th>{{new Date(day.dt*1000).toLocaleDateString()}}</th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <td>Low/High</td>
            <td>{{day.temp.min}} / {{day.temp.max}} &deg;F</td>
          </tr>
          <tr>
            <td>Humidity</td>
            <td>{{day.humidity}}%</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</div>
</template>
<script>
import Skycon from 'vue-skycons';
export default {
  name: 'Forecast',
  components:{
    Skycon
  },
  props:{
    weather:Object,
    location:Object
  }
}
</script>
<style scoped>
  .container-flex{
    display:flex;
    flex-wrap:wrap;
    align-items: center;
    justify-content: center;
  }
  .item{
    margin:.5rem;
    padding:.5rem;
  }
</style>
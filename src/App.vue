<template>
<main>
  <Current :weather="weather.current" :location="location" :wxIcon="wxIcon" />
</main>
</template>
<script>
  import axios from 'axios';
  import moment from 'moment-timezone';
  import geoFind from 'reverse-geocode';
  import Current from './components/Current'
export default {
  name: 'Weather',
  components:{
    Current
  },
  data:()=>{
    return{
      weather:{current:{}},
      location:{},
      iconLookup:{
        "Thunderstorm":"rain",
        "Drizzle":"rain",
        "Rain":"rain",
        "Snow":"snow",
        "Clear":"clear",
        "Atmosphere":"fog",
        "Clouds":"cloudy"
      },
      wxIcon:""
    }
  },
  methods:{
    getF:(K)=>Math.round(((K-273.15)*1.8)+32),
    getHg:(P)=>Math.round(P*0.029529983071445),
    getDateTime:(T)=>moment(new Date(T*1000)).format('MM/DD/YYYY h:mm A'),
    getTime:(T)=>moment(new Date(T*1000)).format('h:MM A'),
    getLoc:(lat,lon)=>geoFind.lookup(lat,lon,'us'),
    getIcon:(weather)=>{

    }
  },
  mounted(){
    navigator.geolocation.getCurrentPosition(async(pos)=>{
      const response = await axios.get(`https://api.openweathermap.org/data/3.0/onecall?lat=${pos.coords.latitude}&lon=${pos.coords.longitude}&appid=${process.env.VUE_APP_API_KEY}`)
      this.weather = response.data
      this.wxIcon = this.iconLookup[this.weather.current.weather[0].main]
      if(this.wxIcon == "clear"){
        new Date()>=new Date(this.weather.current.sunset*1000) ?  this.wxIcon = "clear-night" : this.wxIcon = "clear-day"
      }

      this.weather.current.dt = this.getDateTime(this.weather.current.dt)
      this.weather.current.sunrise = this.getTime(this.weather.current.sunrise)
      this.weather.current.sunset = this.getTime(this.weather.current.sunset)
      this.weather.current.temp = this.getF(this.weather.current.temp)
      this.weather.current.feels_like = this.getF(this.weather.current.feels_like)
      this.weather.current.dew_point = this.getF(this.weather.current.dew_point)
      this.weather.current.pressure = this.getHg(this.weather.current.pressure)
      this.weather.daily.shift()
      for(let i=0; i<this.weather.daily.length; i++){
        this.weather.daily[i].temp.max = this.getF(this.weather.daily[i].temp.max)
        this.weather.daily[i].temp.min = this.getF(this.weather.daily[i].temp.min)
      }
      this.location = this.getLoc(pos.coords.latitude,pos.coords.longitude)
    },(err)=>alert('Your browser does not support this app!'))
  }
}
  
</script>
<style>
*{
    font-family: system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
}
.container{
    margin-bottom:.5rem;
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
.text-underline{
  text-decoration: underline;
}
.text-white{
  color:white;
}
</style>
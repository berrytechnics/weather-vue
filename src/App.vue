<template>
	<Transition>
		<Loading v-if="isLoading" class="loadingComponent"/>
	</Transition>
	<div class="container">
		<Current
			class="component currentComponent"
			:weather="weather.current"
			:location="location"
			:wxIcon="wxIcon" />
		<Precipitation
			class="component precipitationComponent"
			:weather="precip" />
	</div>
	<div class="container">
		<Forecast 
			class="component forecastComponent" 
			:weather="weather.daily" />
	</div>
</template>
<script>
	import axios from "axios"
	import moment from "moment-timezone"
	import geoFind from "reverse-geocode"
	import Forecast from "./components/Forecast"
	import Current from "./components/Current"
	import Precipitation from './components/Precipitation'
	import Loading from './components/Loading'
	export default {
		name: "Weather",
		components: {
			Current,
			Forecast,
			Precipitation,
			Loading
		},
		data() {
			return {
				isLoading:true,
				weather: { current: {} },
				location: {},
				wxIcon: "",
				precip:{}
			}
		},
		methods: {
			getF: K => Math.round((K - 273.15) * 1.8 + 32),
			getHg: P => Math.round(P * 0.029529983071445 * 100) / 100,
			getDateTime: T => moment(new Date(T * 1000)).format("MM/DD/YYYY h:mm A"),
			getTime: T => moment(new Date(T * 1000)).format("h:MM A"),
			getLoc: (lat, lon) => geoFind.lookup(lat, lon, "us"),
			getWxIcon: weather => {
				const lookup = {
					Thunderstorm: "rain",
					Drizzle: "rain",
					Rain: "rain",
					Snow: "snow",
					Clear: "clear",
					Atmosphere: "fog",
					Clouds: "cloudy",
				}
				return lookup[weather]
			},
		},
		mounted() {
			navigator.geolocation.getCurrentPosition(
				async pos => {
					const response = await axios.get(
						`https://api.openweathermap.org/data/3.0/onecall?lat=${pos.coords.latitude}&lon=${pos.coords.longitude}&appid=${process.env.VUE_APP_API_KEY}`
					)
					this.weather = response.data
					this.wxIcon = this.getWxIcon(this.weather.current.weather[0].main)
					if (this.wxIcon == "clear") {
						new Date() >= new Date(this.weather.current.sunset * 1000)
							? (this.wxIcon = "clear-night")
							: (this.wxIcon = "clear-day")
					}
					this.weather.current.dt = this.getDateTime(this.weather.current.dt)
					this.weather.current.sunrise = this.getTime(this.weather.current.sunrise)
					this.weather.current.sunset = this.getTime(this.weather.current.sunset)
					this.weather.current.temp = this.getF(this.weather.current.temp)
					this.weather.current.feels_like = this.getF(this.weather.current.feels_like)
					this.weather.current.dew_point = this.getF(this.weather.current.dew_point)
					this.weather.current.pressure = this.getHg(this.weather.current.pressure)
					this.weather.current.min = this.getF(this.weather.daily[0].temp.min)
					this.weather.current.max = this.getF(this.weather.daily[0].temp.max)
					this.weather.daily.shift()
					for (let i = 0; i < this.weather.daily.length; i++) {
						this.weather.daily[i].pressure = this.getHg(this.weather.daily[i].pressure)
						this.weather.daily[i].temp.max = this.getF(this.weather.daily[i].temp.max)
						this.weather.daily[i].temp.min = this.getF(this.weather.daily[i].temp.min)
					}
					let precipArr = []
					for(let i=0;i<this.weather.minutely.length;i++){
						precipArr.push(this.weather.minutely[i].precipitation*100)
					}
					this.precip = {data:precipArr}
					this.location = this.getLoc(pos.coords.latitude, pos.coords.longitude)
					this.isLoading=false
				},
				err => alert("Your browser does not support this app!")
			)
		},
	}
</script>
<style>
/* screen break */
@media(min-width:768px){
	.container {
	display: flex;
	flex-wrap: wrap;
	justify-content: center;
	}
}
.component {
	color: #b2ca59;
	font-family: Roboto, system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", Oxygen, Ubuntu,
		Cantarell, "Open Sans", "Helvetica Neue", sans-serif;
	background-color: rgb(49, 46, 46);
	border-radius: 1rem;
	padding: 1rem;
	white-space: nowrap;
	margin:.5rem;

}
.currentComponent{
	flex:1;
}
.forecastComponent{
	flex:6;
}
.precipitationComponent, .precipitationComponent canvas{
	flex:3;
	max-height:50vh;
	padding-bottom:3rem;
}
.text-center {
	text-align: center;
}
.text-right {
	text-align: right;
}
.text-underline {
	text-decoration: underline;
}
/* Vue transitions \/  */
.v-enter-active,
.v-leave-active {
  transition: opacity 0.2s ease;
}
.v-enter-from,
.v-leave-to {
  opacity: 0;
}
</style>

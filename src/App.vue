<template>
	<main class="container">
		<Current
		class="component currentComponent"
		:weather="weather.current"
		:location="location"
		:wxIcon="wxIcon" />
		<Forecast 
			class="component forecastComponent" 
			:weather="weather.daily" 
			location="location" />
	</main>
</template>
<script>
	import axios from "axios"
	import moment from "moment-timezone"
	import geoFind from "reverse-geocode"
	import Forecast from "./components/Forecast"
	import Current from "./components/Current"
	export default {
		name: "Weather",
		components: {
			Current,
			Forecast,
		},
		data() {
			return {
				weather: { current: {} },
				location: {},
				wxIcon: "",
			}
		},
		methods: {
			getF: K => Math.round((K - 273.15) * 1.8 + 32),
			getHg: P => Math.round(P * 0.029529983071445),
			getDateTime: T => moment(new Date(T * 1000)).format("MM/DD/YYYY h:mm A"),
			getTime: T => moment(new Date(T * 1000)).format("h:MM A"),
			getLoc: (lat, lon) => geoFind.lookup(lat, lon, "us"),
			getWxIcon: weather => {
				let lookup = {
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
						this.weather.daily[i].temp.max = this.getF(this.weather.daily[i].temp.max)
						this.weather.daily[i].temp.min = this.getF(this.weather.daily[i].temp.min)
					}
					this.location = this.getLoc(pos.coords.latitude, pos.coords.longitude)
				},
				err => alert("Your browser does not support this app!")
			)
		}
	}
</script>
<style>
.container {
	border: none;
	border-radius: 1rem;
	display: flex;
	flex-wrap:wrap;
	flex-basis:4;
	justify-content: center;
}
.component{
	color: rgb(178, 202, 89);
	font-family: Roboto, system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", Oxygen, Ubuntu,
		Cantarell, "Open Sans", "Helvetica Neue", sans-serif;
	background-color: rgb(49, 46, 46);
	border-radius:1rem;
	padding:1rem;
	white-space: nowrap;
	margin:.25rem;
}
.currentComponent{
	flex-grow:3;
}
.forecastComponent{
	flex-grow:6;
}
.text-center{
	text-align: center;
}
.text-right{
	text-align: right;
}
.text-underline{
	text-decoration: underline;
}

</style>

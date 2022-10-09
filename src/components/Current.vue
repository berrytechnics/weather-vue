<template>
	<div class="current-weather">
		<div class="header text-center">
				<h1>Current weather</h1>
				<h3>for {{ location.city }}, {{ location.state_abbr }} ({{ location.zipcode }})</h3>
			</div>
		<div class="container">
			<div class="box">
				<div>
					<Skycon
						size="150"
						speed="3"
						color="#ffff00"
						v-if="wxIcon == 'clear-day'"
						condition="clear-day" />
					<Skycon
						size="150"
						speed="3"
						color="#ccccaa"
						v-if="wxIcon == 'clear-night'"
						condition="clear-night" />
					<Skycon size="150" speed="3" color="#aaaaaa" v-if="wxIcon == 'fog'" condition="fog" />
					<Skycon size="150" speed="3" color="#999999" v-if="wxIcon == 'rain'" condition="rain" />
					<Skycon size="150" speed="3" color="#ffffff" v-if="wxIcon == 'snow'" condition="snow" />
					<Skycon size="150" speed="3" color="#cccccc" v-if="wxIcon == 'cloudy'" condition="cloudy" />
					<h1 class="text-center">
						<span class="text-underline currentTemp">{{ weather.temp }}&deg;<sup>F</sup></span>
					</h1>
				</div>
			</div>
			<div class="box">
				<div>
					<table>
						<tbody>
							<tr>
								<td class="text-right">Sunrise:</td>
								<td>{{ weather.sunrise }}</td>
							</tr>
							<tr>
								<td class="text-right">Sunset:</td>
								<td>{{ weather.sunset }}</td>
							</tr>
							<tr>
								<td class="text-right">Low / High:</td>
								<td>{{ weather.min }} / {{ weather.max }} &deg;F</td>
							</tr>
							<tr>
								<td class="text-right">Feels Like:</td>
								<td>{{ weather.feels_like }}&deg;F</td>
							</tr>
							<tr>
								<td class="text-right">Humidity:</td>
								<td>{{ weather.humidity }}%</td>
							</tr>
							<tr>
								<td class="text-right">Dew Point:</td>
								<td>{{ weather.dew_point }}&deg;F</td>
							</tr>
							<tr>
								<td class="text-right">Pressure:</td>
								<td>{{ weather.pressure }}"/Hg</td>
							</tr>
						</tbody>
					</table>
				</div>
			</div>

		</div>
	</div>
</template>
<script>
	import Skycon from "vue-skycons"
	export default {
		name: "Current",
		components: {
			Skycon,
		},
		props: {
			weather: Object,
			location: Object,
			wxIcon: String,
		},
	}
</script>
<style scoped>
	table {
		margin: auto;
	}
	canvas {
		display: block;
		margin: auto;
	}
	.box{
		display:flex;
		justify-content: space-around;
	}
	.currentTemp{
		line-break: nowrap;
		display:block;
		margin-top:-3rem;
	}
</style>

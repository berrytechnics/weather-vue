<template>
	<div class="forecast">
		<div class="header">
			<h1 class="text-center">Forecast</h1>
			<div class="container">
				<div class="box" v-for="forecast in weather">
					<h4 class="text-center">{{ this.lookup[new Date(forecast.dt * 1000).getDay()] }}</h4>
					<h5 class="text-center">{{ new Date(forecast.dt * 1000).toLocaleDateString() }}</h5>
					<div class="img">
						<img :src="iconURL + forecast.weather[0].icon + '@2x.png'" /><br />
						<table>
							<tbody>
								<tr>
									<td>Low-High</td>
									<td class="text-center">
										{{ forecast.temp.min }}&deg;-{{ forecast.temp.max }}&deg;
									</td>
								</tr>
								<tr>
									<td>Humidity</td>
									<td class="text-center">{{ forecast.humidity }}%</td>
								</tr>
								<tr>
									<td>Pressure</td>
									<td class="text-center">{{ forecast.pressure }}"/Hg</td>
								</tr>
							</tbody>
						</table>
					</div>
				</div>
			</div>
		</div>
	</div>
</template>
<script>
	export default {
		name: "Forecast",
		components: [],
		props: {
			weather: Object
		},
		data() {
			return {
				iconURL: "http://openweathermap.org/img/wn/",
				lookup: ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"],
			}
		},
	}
</script>
<style scoped>
	.container {
		display: flex;
		flex-wrap: wrap;
		gap: .5rem;
		justify-content: space-around;
	}
	.box {
		border: 1px black solid;
		border-radius: 1rem;
		padding: .3rem;
		box-shadow: 0.5rem 0.5rem 0.8rem rgba(0, 0, 1, 0.319);
	}
	img {
		display: block;
		margin: auto;
	}
</style>
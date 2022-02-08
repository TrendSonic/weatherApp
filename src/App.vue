<template>
  <div id="app" 
		:class="[(typeof weather.main != 'undefined' && weather.main.temp > 30 ? 'warm' : ''), (typeof weather.main != 'undefined' && weather.main.temp > 0 ? 'middleCold' : ''), (typeof weather.main != 'undefined' && weather.main.temp > 16 ? 'middleWarm' : '')
		]">
		<div class="app">
			<div class="search-box">
				<input 
					type="text" 
					class="search-bar" 
					placeholder="Search..."
					v-model="place"
					@keypress.enter="fetchWeather"
					>
			</div>

			<div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
				<div class="location-box">
					<div class="location">
						{{ weather.name }}, {{ weather.sys.country }}
					</div>
					<div class="date">
						{{ day }}, {{ date }} {{ month }} {{ year }}
					</div>
				</div>

				<div class="weather-box">
					<div class="temp">
						{{ Math.round(weather.main.temp) }}°C
					</div>
					<div class="weather">
						<p class="main-desc">{{ weather.weather[0].main }}</p>
						<p class="desc">{{ weather.weather[0].description }}</p>
					</div>
				</div>
			</div>
		</div>
	</div>	
</template>

<script>
import { ref } from '@vue/reactivity'

export default {
  name: 'App',
	// new
	setup() {
		const api_key = 'f971864f8a30e6004b27a97750f2b6ed'
		const url = 'https://api.openweathermap.org/data/2.5/'

		const place = ref('')
		const weather = ref({})
		let day = ref('')
		let date = ref('')
		let month = ref('')
		let year = ref('')

		// fetching data
		const fetchWeather = () => {
			fetch(`${url}weather?q=${place.value}&units=metric&APPID=${api_key}`)
				.then(res => res.json())
				.then(setResults)
		}

		const setResults = (results) => {
			weather.value = results
			place.value = ''
			getCurrentDate()
		}

		// get date
		const getCurrentDate = () => {
			const d = new Date()
			let months = [ 'January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December']
			let days = ['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursady', 'Friday', 'Saturday']

			day.value = days[d.getDay()]
			date.value = d.getDate()
			month.value = months[d.getMonth()]
			year.value = d.getFullYear()
			console.log(day.value, date.value, month.value, year.value)
		}

		return { place, fetchWeather, weather, day, date, month, year }
	}
}
</script>

<style>
* {
	padding: 0;
	margin: 0;
	box-sizing: border-box;
}

#app {
	font-family: 'montserrat', sans-serif;
	height: 100vh;
	background-image: url('./assets/cold.jpg');
	background-repeat: no-repeat;
	background-size: cover;
	background-position: bottom;
	transition: 0.4s;
	position: relative;
}
#app.middleCold {
	background-image: url('./assets/middleCold.jpg');
}
#app.middleWarm {
	background-image: url('./assets/middleWarm.jpg');
}
#app.warm {
	background-image: url('./assets/warm.jpg');
}

.app {
	height: 100vh;
	width: 100%;
	left: 0;
	top: 0;
	position: absolute;
	background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.25), rgba(0, 0, 0, 0.75));
	transition: 0.4s;
}

#app.warm .app {
	background-image: none;
}

.search-box {
	width: 100%;
	margin-bottom: 30px;
	padding: 25px;
}

.search-box .search-bar {
	display: block;
	width: 75%;
	padding: 15px;
	font-size: 20px;
	color: #313131;
	margin: 0 auto;

	appearance: none;
	border: none;
	outline: none;
	background: none;

	box-shadow: 0 0 8px rgba(0, 0, 0, 0.25);
	background: rgba(255, 255, 255, 0.25);
	border-radius: 0 16px 0 16px;
	transition: 0.4s;
}

.search-box .search-bar:hover {
  background-color: rgba(255, 255, 255, 0.5);
	border-radius: 8px;
}

.search-box .search-bar:focus {
	box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.75);
  border-radius: 16px 0px 16px 0px;
}

.location-box .location {
  color: #fff;
  font-size: 40px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}

.location-box .date {
  color: #fff;
  font-size: 22px;
  font-weight: 300;
  font-style: italic;
  text-align: center;
	margin-bottom: 15px;
}

.weather-box {
  text-align: center;
}

.weather-box .temp {
  display: inline-block;
  padding: 10px 35px;
  color: #fff;
  font-size: 102px;
  font-weight: 900;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color:rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0px;
  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.weather-box .weather {
  color: #fff;
  font-size: 48px;
  font-weight: 600;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.desc {
	font-size: 22px;
	margin-top: 10px;
	font-weight: normal;
}
</style>

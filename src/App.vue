<template>
	<div className="wrapper">
		<h1>Погодное приложение</h1>
		<p>Узнать погоду в {{ city == '' ? 'вашем городе' : cityName }}</p>
		<input type="text" v-model="city" placeholder="Введите ваш город" />
		<button v-if="city != ''" @click="getWeather()">Узнать погоду</button>
		<button disabled v-else>Узнать погоду</button>

		<p class="error">{{ error }}</p>

		<div v-if="info != null">
			<p>{{ showTemp }}</p>
			<p>{{ showFellLike }}</p>
			<p>{{ showMax }}</p>
			<p>{{ showMin }}</p>
		</div>
	</div>
</template>

<script>
import axios from 'axios'
export default {
	data() {
		return {
			city: '',
			error: '',
			info: null,
		}
	},
	computed: {
		cityName() {
			return '<' + this.city + '>'
		},
		showTemp() {
			return 'Температура: ' + this.info.main.temp
		},
		showFellLike() {
			return 'Ощущается как: ' + this.info.main.feels_like
		},
		showMax() {
			return 'Максимум: ' + this.info.main.temp_max
		},
		showMin() {
			return 'Минимум: ' + this.info.main.temp_min
		},
	},
	methods: {
		getWeather() {
			if (this.city.trim().length <= 2) {
				this.error = 'В названии должно быть больше 2 символов'
				return false
			}

			this.error = ''

			axios
				.get(
					`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=5f2513ac42aaf5b0e0d17121d826f73b`
				)
				.then(res => (this.info = res.data))
		},
	},
}
</script>

<style scoped>
.error {
	color: red;
}

.wrapper {
	width: 900px;
	height: 500px;
	border-radius: 50px;
	background-color: #1f0f24;
	text-align: center;
	padding: 20px;
	color: white;
}

.wrapper h1 {
	margin-top: 50px;
}

.wrapper p {
	margin-top: 20px;
}

.wrapper input {
	border: 0;
	border-bottom: 2px solid #110813;
	background: transparent;
	margin-top: 20px;
	color: #fcfcfc;
	outline: none;
	font-size: 14px;
	padding: 5px 8px;
}

.wrapper input:focus {
	border-bottom-color: #6e2d7d;
}

.wrapper button {
	background: #e3bc4b;
	color: #fff;
	border-radius: 10px;
	border: 2px solid #b99935;
	padding: 10px 15px;
	margin-left: 20px;
	cursor: pointer;
	transition: transform 500ms ease;
}

.wrapper button:hover {
	transform: scale(1.1) translateY(-3px);
}

.wrapper button:disabled {
	background: #7a6628;
	cursor: not-allowed;
}
</style>

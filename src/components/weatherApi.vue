<template>
  <div class="container">
    <h1>weather app</h1>

    <div class="search">
      <label for="search">search city </label>
      <input type="text" id="search" v-model="city" @keyup.enter="getWheater" />
      <button @click="getWheater">search</button>
    </div>

    <div v-if="weather">
      <h3>city: {{ city }}</h3>
      <h3>the description: {{ weather.description }}</h3>
      <h3>the temperature: {{ weather.temp }}%</h3>
      <h3>the humidty: {{ weather.humidity }}</h3>
      <h3>the temp-max: {{ weather.max }}</h3>
      <h3>the temp-min: {{ weather.min }}</h3>
    </div>

    <div v-if="error">
      <h1 class="error">{{ error }}</h1>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const city = ref('')
const weather = ref(null)
const error = ref('')

const getWheater = async () => {
  try {
    const response = await fetch(
      `https://api.openweathermap.org/data/2.5/weather?q=${city.value}&appid=294b2596c5b0bc802da947fa1d12a5db&units=metric`
    )
    const data = await response.json()

    weather.value = {
      description: data.weather[0].description,
      temp: data.main.temp,
      humidity: data.main.humidity,
      max: data.main.temp_max,
      min: data.main.temp_min
    }
    error.value = null
  } catch (err) {
    weather.value = false
    error.value = 'Unable to get weather data for this city.'
  }
}
</script>

<style scoped>
.container {
  font-family: 'Roboto', sans-serif;
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
  background-color: #fff;
  border-radius: 8px;
  box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.1);
  display: flex;
  flex-direction: column;
  align-items: center;
}

h1 {
  color: #444;
  font-size: 2.5em;
}

.search {
  display: flex;
  justify-content: space-between;
  width: 100%;
  margin-bottom: 20px;
}

.search label {
  flex-grow: 1;
  text-align: left;
  font-size: 1.2em;
  color: #555;
}

.search input {
  flex-grow: 2;
  padding: 10px;
  margin-right: 10px;
  border-radius: 4px;
  border: 1px solid #ddd;
  font-size: 1em;
}

.search button {
  padding: 10px 20px;
  background-color: #008cba;
  color: white;
  border: none;
  cursor: pointer;
  border-radius: 4px;
  font-size: 1em;
}

.weather-info {
  background-color: #f3f3f3;
  padding: 20px;
  border-radius: 4px;
  box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.1);
  width: 100%;
}

.weather-info h1,
.weather-info h2 {
  color: #333;
  font-size: 1.5em;
}

.weather-info img {
  display: block;
  margin: 0 auto;
  width: 50px;
  height: 50px;
}

.error {
  color: red;
}
</style>

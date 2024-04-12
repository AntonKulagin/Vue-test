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
      return '«' + this.city + '»'
    },
    showTemp() {
      return 'Температура ' + this.info.main.temp
    },
    showMinTemp() {
      return 'Температура min ' + this.info.main.temp_min
    },
    showMaxTemp() {
      return 'Температура max ' + this.info.main.temp_max
    },
  },
  methods: {
    getWeather() {
      if (this.city.trim().length < 2) {
        this.error = 'Ошибочка'
        return false
      }
      this.error = ''
      axios
        .get(
          `https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=b19eedaed530e93d00ac7d38636c90e3`
        )
        .then((res) => (this.info = res.data))
        .catch((error) => (this.error = error.message))
    },
  },
}
</script>

<template>
  <div className="wrapper">
    <div className="weather">
      <h2>Погодное приложение</h2>
      <h3>Погода в городе {{ city === '' ? 'N' : cityName }}</h3>
      <input type="text" v-model="city" placeholder="название города" />
      <button v-if="city.length === 0" disabled>Получить данные</button>
      <button v-else @click="getWeather()">Получить данные</button>
      <p className="error">{{ error }}</p>
      <div v-if="info !== null">
        <p>{{ showTemp }}</p>
        <p>{{ showMinTemp }}</p>
        <p>{{ showMaxTemp }}</p>
      </div>
    </div>
  </div>
</template>

<style scoped>
.error {
  color: red;
}
.wrapper {
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
}
.weather {
  width: 900px;
  height: 500px;
  background-color: #fff;
  border-radius: 40px;
  padding: 15px 30px;
  display: flex;
  flex-direction: column;
  align-items: center;
}

h2,
h3,
input,
button {
  margin-bottom: 10px;
}

input {
  border: 0;
  border-bottom: 1px solid #ccc;
}
input:focus {
  outline: 0;
  border-bottom: 1px solid #d64400;
}
button {
  padding: 5px;
  cursor: pointer;
}
button:disabled {
  cursor: not-allowed;
}
</style>

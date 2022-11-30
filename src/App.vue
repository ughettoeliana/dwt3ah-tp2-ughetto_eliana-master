<template>
  <main :class="weatherClass">
    <CitySearchbar :on-search="onSearch"></CitySearchbar>
    <div id='errorText' class="errorText" v-if="hasError">
      <p>We didn't find the city '{{ searchQuery }}', try again with another one</p>
    </div>
    <div id='home' class="weather-wrap" v-if="hasWeather">
      <div class="location-box">
        <h1 class="location">
          {{ activeCity?.name }}, {{ activeCity?.country }}
        </h1>
        <div class="date">{{ dateBuilder() }}</div>
      </div>
      <div class="weather-box">
        <div class="temp">{{ activeCity?.temp }}°c</div>
        <div class="weather">{{ activeCity?.weather }}</div>
      </div>
    </div>
    <section id="saved">
    </section>
    <hr>
    <h2 class="white-text">Recent searchs</h2>
    <section id='recents' class="cityHistory-wrap">
      <CityCard :city-history="cityHistory"></CityCard>
    </section>
  </main>
</template>

<script>
import CitySearchbar from './components/CitySearchbar.vue'
import CityCard from './components/CityCard.vue'

export default {
  name: 'App',
  components: {
    CitySearchbar,
    CityCard,
  },
  data: () => {
    return {
      searchQuery: '',
      cityHistory: [],
      hasError: false,
      searchList: []
    }
  },

  methods: {

    parseWeatherResponse(res) {
      return {
        id: res.id,
        name: res.name,
        country: res.sys.country,
        temp: Math.round(res.main.temp),
        weather: res.weather[0].main,
      }
    },

    dateBuilder() {
      const months = [
        'January',
        'Februery',
        'March',
        'April',
        'May',
        'June',
        'July',
        'September',
        'October',
        'November',
        'December',
      ];
      const days = [
        'Sunday',
        'Monday',
        'Tuesday',
        'Wednesday',
        'Thursday',
        'Friday',
        'Saturday',
      ];
      let d = new Date();
      let day = days[d.getDay()];
      let month = months[d.getMonth()];
      let date = d.getDate();
      let year = d.getFullYear();

      return `${day} ${date} ${month} ${year}`;
    },

    async onSearch(searchValue) {
      const rawCity = await this.fetchWeather(searchValue);
      if (rawCity.name == '' || rawCity.name == null) {
        this.searchQuery += searchValue;
        this.hasError = true;
        this.searchQuery += ' ';
        console.log(this.searchQuery);
        return;
      }

      // Borramos la busqueda anterior de la misma ciudad.
      this.cityHistory = this.cityHistory.filter(item => item.id !== rawCity.id);
      const newCity = this.parseWeatherResponse(rawCity);
      this.cityHistory.push(newCity);
      this.hasError = false;
      this.searchQuery = '';
    },

    async fetchWeather(searchQuery) {
      const baseUrl = 'https://api.openweathermap.org/data/2.5/weather?q=';
      const apiKey = 'ba9d29841e8cf91d17e903bbcc9dc4a1';

      return fetch(`${baseUrl}${searchQuery}&appid=${apiKey}&units=metric`)
        .then(res => res.json())
        .catch(() => { });
    },
  },

  watch: {
    cityHistory(newCity) {
      localStorage.cityHistory = JSON.stringify(newCity);
    },

  },

  mounted() {
    if (localStorage.cityHistory) {
      this.cityHistory = JSON.parse(localStorage.cityHistory);
    }
  },

  computed: {
    hasWeather() {
      return this.cityHistory.length > 0;
    },

    activeCity() {
      return this.cityHistory[this.cityHistory.length - 1];
    },

    weatherClass() {
      return this.activeCity?.temp > 16 ? 'warm' : '';
    },

  },

}
</script>



<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>

<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 16 ? 'warm' : ''">
    <main>
      <div class="search">
        <input type="text" 
               class="search-bar" 
               placeholder="Search...."
               v-model="query" 
               @keypress="fetchWeather"
        />
      </div>

      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>

        <div class="weather-box">
          <div class="temperature">{{ Math.round(weather.main.temp) }}°C</div>
          <div class="weather">{{ weather.weather[0].main }}</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>

export default {
  name: 'App',
  data () {
    return {
      api_key: 'b346aa437f1c6ee082d27c6ecdfb65cc',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {}
    }
  },
  methods: {
    fetchWeather (e) {
      if(e.key == 'Enter') {
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
        .then(res => {
          return res.json();
        }).then(this.setResults);
      }
    },

    setResults(results) {
      this.weather = results;
    },

    dateBuilder () {
      let d = new Date();
      let months = ['January', 'February', 'March', 'April', 'May', 'June',
                    'July', 'August', 'September', 'October', 'November', 'December'];
      let days = ['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday',
                  'Friday', 'Saturday', 'Sunday'];

      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();

      return `${day}, ${date} ${month} ${year}`
    }
  }
}
</script>

<style>
:root {
  font-size: 16px;
}

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'montserrat', sans-serif;
}

#app {
  background-image: url('./assets/cold-bg.jpg');
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
  
  margin: 0 auto;
  max-width: 768px;
}

#app.warm {
  background-image: url('./assets/warm-bg.jpg');
}

main {
  min-height: 100vh;
  padding: 1.5rem;

  background-image: linear-gradient(to bottom, rgba(0,0,0,.25), rgba(0,0,0,.75));
}

.search {
  width: 100%;
  margin-bottom: 2rem;
}

.search .search-bar {
  display: block;
  width: 100%;
  padding: .9rem;
  color: #313131;
  font-size: 1.5rem;
  appearance: none;
  border: none;
  outline: none;
  background: none;
  background-color: rgba(255,255,255,.5);
  border-radius: .25rem;
  box-shadow: 0 0 1rem rgba(0,0,0,.25);
  transition: 0.4s;
}

.search .search-bar:focus {
  background-color: rgba(255,255,255,.75);
  border-radius: 2rem;
}

.location-box .location {
  color:#fff;
  font-size: 2rem;
  font-weight: 500;
  text-align: center;
  text-shadow: .01rem .06rem rgba(0,0,0,.25);
}

.location-box .date {
  color:#fff;
  font-size: 1.8rem;
  font-weight: 300;
  font-style: italic;
  text-align: center;
}

.weather-box {
  text-align: center;
}

.weather-box .temperature {
  display: inline-block;
  padding: .7rem 1.5rem;
  color: #fff;
  font-size: 9rem;
  font-weight: 900;
  text-shadow: .01rem .06rem rgba(0,0,0,.25);
  background-color: rgba(255,255,255,.2);
  border-radius: 1rem;
  margin: 2rem 0;
  box-shadow: 0 0 1rem rgba(0,0,0,.25);
}

.weather-box .weather {
  color: #fff;
  font-size: 3rem;
  font-weight: 700;
  font-style: italic;
  text-shadow: .01rem .06rem rgba(0,0,0,.25);
}
</style>

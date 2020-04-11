<template>
  <div 
    id="app" 
    :class="typeof weather.main != 'undefined' && weatherImage()"
  >
    <main>
      <div class="search-box">
        <input 
          type="text" 
          class="search-bar" 
          placeholder="Rechercher..."
          v-model="query"
          @keypress="fetchWeather"
        />
      </div>

      <div class="weather-wrap" v-if=" typeof weather.main != 'undefined'">
        <div class="location">{{ weather.name }}, {{ weather.sys.country }} </div>
        <div class="date"> {{ dateBulder() }}</div>
      </div>

      <div class="weather-box" v-if=" typeof weather.main != 'undefined'">
        <div class="temp"> {{ Math.round(weather.main.temp) }}°C</div>
        <div class="weather"> {{ weather.weather[0].main}}</div>
      </div>
    </main>
  </div>
  
</template>

<script>
export default {
  name: 'app',
  data() {
    return {
      api_key: '7e055336eece3acb4821e7493f40b5e1',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {},
      sun: 'sun',
      rain: 'rain',
      cold: 'cold'
    }
  },
  methods: {
    fetchWeather (e) {
      if (e.key == "Enter") {
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
          .then(res => {
            return res.json();
          }).then(this.setResults);
      }
    },
    setResults (results) {
      this.weather = results;
    },
    dateBulder () {
      let d = new Date();
      let months = ['Janvier', 'Février', 'Mars', 'Avril', 'Mai', 'Juin', 'Juillet', 'Août', 'Septembre', 'Octobre', 'Novembre', 'Décembre'];
      let days = ['Lundi', 'Mardi', 'Mercredi', 'Jeudi', 'Vendredi', 'Samedi', 'Dimanche'];

      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();

      return `${day} ${date} ${month} ${year} `
    },
    weatherImage () {
      
      if(this.weather.main.temp > 16 && this.weather.weather[0].main == "Clear" ) {
        return this.sun
      }
      if(this.weather.weather[0].main == "Clouds") {
        return this.rain
      }
      if(this.weather.main.temp > 5) {
        return this.cold
      }
      else {
        return ''
      }
      }
    }
  }

</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'montserra', sans-serif;
}

#app {
  background-image: url('./assets/cold-bg.jpg');
  background-size: cover;
  background-position: bottom;
  transition: .4s
}
#app.sun {
  background-image: url(./assets/sun-bg.jpg);
}
#app.rain {
  background-image: url(./assets/rain-bg.jpg);
}
#app.night {
  background-image: url(./assets/night-bg.jpg);
}
#app.cold {
  background-image: url('./assets/cold-bg.jpg');
}
main {
  min-height: 100vh;
  padding: 25px;

  background-image: linear-gradient(to bottom, rgba(0,0,0,0.25), rgba(0,0,0,0.75))
}

.search-box {
  width: 100%;
  margin-bottom: 30px;
}

.search-bar {
  display: block;
  width: 100%;
  padding: 15px;

  color: #313131;
  font-size: 20px;
  
  appearance: none;
  border: none;
  outline: none;
  background: none;

  box-shadow: 0px 0px 8px rgba(0,0,0,0.25);
  background-color: rgba(255,255,255, 0.5);
  border-radius: 0px 16px 0px 16px;
  transition: 0.4s;
}
.search-bar:focus {
  box-shadow: 0px 0px 16px rgba(0,0,0,0.25);
  background-color: rgba(255,255,255, 0.75);
  border-radius: 16px 0px 16px 0px;
}

.location-box, .location {
  color: #fff;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0,0,0,0.25);
}

.location-box, .date {
  color: #fff;
  font-size: 20px;
  font-weight: 300;
  font-style: italic;
  text-align: center;
}

.temp {
  display: inline-block;
  padding: 10px 25px;
  color: #fff;
  font-size: 102px;
  font-weight: 900;

  text-shadow: 3px 6px rgba(0,0,0,0.25);
  background-color: rgba(255,255,255, 0.5);
  border-radius: 16px;
  margin: 30px 0px;

  box-shadow: 3px 6px rgba(0,0,0,0.25);
}

.weather-box, .weather {
  width: 100%;
  color: #fff;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-align: center;
  text-shadow: 3px 6px rgba(0,0,0,0.25);
}
</style>

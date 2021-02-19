<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 16 ? 'warm' : ''">
    <main>
      <div class="search-box">
        <input 
          type="text" 
          class="search-bar" 
          placeholder="Search City..."
          v-model="query"
          @keypress="fetchWeather"
        />
      </div>
      <div v-if="weather.main == null">
        <div class="weather-box">
          <img class="logo" src="./assets/def.png" />
          <img class="name" src="./assets/name.png" />
          <img class="used" src="./assets/vue.png" />
          <img class="used" src="./assets/node.png" />
          <img class="used" src="./assets/html.png" />
          <img class="used" src="./assets/css.png" />
          <img class="used" src="./assets/js.png" />
        </div>
      </div>
      <div v-else>
      <div id="climate" :class="typeof weather.main == 'warm'"></div>
      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
          <div class="date">({{ dateBuilder() }})</div>
        </div>
      
        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp) }}°c</div>
          <div class="weather status">{{ weather.weather[0].main }}</div>
          <div v-if="weather.weather[0].main === 'Warm'">
            <img class="gif" src="./assets/warm.gif" />
          </div>
          <div v-else-if="weather.weather[0].main === 'Rain'">
            <img class="gif" src="./assets/rain.gif" />
          </div>
          <div v-else-if="weather.weather[0].main === 'Snow'">
            <img class="gif" src="./assets/snow.gif" />
          </div>
          <div v-else-if="weather.weather[0].main === 'Clear'">
            <img class="gif" src="./assets/clear.gif" />
          </div>
          <div v-else>
            <img class="gif" src="./assets/cloud.gif" />
          </div>
          <!-- <div class="weather status">{{ weather.weather[0].main }}</div> -->
        </div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      api_key: '244e0ab1fe7ea52e93bea0110a11c2db',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {}
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
    dateBuilder () {
      let d = new Date();
      let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
      let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];

      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();

      return `${day} ${date} ${month} ${year}`;
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
  font-family: 'montserrat', sans-serif;
}

#app {
  background-image: url('./assets/default.jpg');
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}

#app.warm {
  background-image: url('./assets/warm-bg.jpg');
}

main {
  min-height: 100vh;
  padding: 25px;

  background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.25), rgba(0, 0, 0, 0.75));
}

.search-box {
  margin-bottom: 30px;
  margin-top: 30px;
}

.search-box .search-bar {
  display: block;
  margin-left: auto;
  margin-right: auto;
  
  width: 100%;
  max-width: 600px;
  padding: 15px;
  
  color: #313131;
  font-size: 20px;

  appearance: none;
  border:none;
  outline: none;
  background: none;

  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 0px 16px 0px 16px;
  transition: 0.4s;
}

.search-box .search-bar:focus {
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.75);
  border-radius: 16px 0px 16px 0px;
}

.location-box .location {
  color: rgb(1, 196, 255);
  font-size: 42px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}

.location-box .date {
  color: #FFF;
  font-size: 20px;
  font-weight: 300;
  font-style: italic;
  text-align: center;
}

.weather-box {
  text-align: center;
}

.weather-box .temp {
  margin: auto;
  padding: 10px 10px 10px 10px;
  margin-top: 20px;
  max-width: 400px;
  min-width: 100px;
  display: inherit;
  color: #FFF;
  font-size:10vw;
  font-weight: 900;

  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color:rgba(255, 255, 255, 0.25);
  border-radius: 16px;

  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.weather-box .weather {
  color: #FFF;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.weather-box .status {
  padding-top: 20px;
  font-size: 32px;
  text-shadow: 3px 6px rgba(243, 236, 236, 0.25);
}

.logo {
  position: relative;
  margin: auto;
  padding: 10px 10px 10px 10px;
  margin-top: 20px;
  margin-bottom: 50px;
  max-width: 600px;
  height: 100%;
  width: 100%;
  display: inherit;
  object-fit: cover;  
}


.gif {
  position: relative;
  margin: auto;
  max-width: 600px;
  height: 100%;
  width: 100%;
  display: inherit;
  object-fit: cover;  
}

.name {
  margin: auto;
  padding: 10px 10px 10px 10px;
  margin-top: 20px;
  margin-bottom: 50px;
  max-width: 400px;
  min-width: 100px;
  display: inherit;
  float: center;
  padding-left: 20px;
  width: 70%;  
  height: 100%;  
  object-fit: cover;  
}

.used {
  width: 100px;  
  height: 100px; 
}
</style>

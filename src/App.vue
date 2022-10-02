<template>
  <div
    id="app"
    :class="
      typeof weather.main != 'undefined' && weather.main.temp > 16
        ? 'warmWeather'
        : ''
    "
  >
    <main>
      <div class="search-box">
        <input
          type="text"
          class="search-bar"
          placeholder="search..."
          v-model="query"
          @keypress="fetchWeather"
        />
      </div>
      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="sunTime">
          <span class="sunRise">
            <i class="fa fa-sun-o"></i>
            {{ new Date(weather.sys.sunrise).toLocaleTimeString('en-GB') }} AM
          </span>
          <span class="sunSet">
            <i class="fa fa-moon-o"></i>
            {{ new Date(weather.sys.sunset).toLocaleTimeString('en-GB') }} PM
          </span>
        </div>
        <div class="location-box">
          <div class="location">
            {{ weather.name }}, {{ weather.sys.country }}
          </div>
        </div>
        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp) }} °C</div>
          <div class="weather">{{ weather.weather[0].main }}</div>
        </div>
      </div>
      <div class="notFound" v-else>Please enter a valid location</div>
    </main>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      api_key: "b5002e866967497bf046bd8a763ef79b",
      url_base: "https://api.openweathermap.org/data/2.5/",
      query: "",
      weather: {},
    };
  },
  methods: {
    fetchWeather(e) {
      if (e.key == "Enter") {
        fetch(
          `${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`
        )
          .then((res) => {
            return res.json();
          })
          .then(this.setResults);
      }
    },
    setResults(results) {
      this.weather = results;
      console.log(results);
    },
  },
};
</script>

<style>
body {
  background-image: url("https://images.unsplash.com/photo-1530908295418-a12e326966ba?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=687&q=80");
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}

#app.warmWeather {
  background-image: url("https://images.unsplash.com/photo-1504386106331-3e4e71712b38?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=2232&q=80");
}

main {
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(
    to bottom,
    rgba(0, 0, 0, 0.25),
    rgba(0, 0, 0, 0.75)
  );
}

.search-box {
  width: 100%;
  margin-bottom: 30px;
}

.search-box .search-bar {
  display: box;
  width: 100%;
  padding: 15px;

  color: #000;
  font-size: 20px;

  appearance: none;
  border: none;
  outline: none;
  background: none;

  background-color: #f7deba;
  border-radius: 25px;
  transition: 0.4s;
}

.search-box .search-bar:focus {
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color: #f5c4c4;
  border-radius: 20px;
}

.location-box .location {
  color: white;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}

.location-box .date {
  color: white;
  font-size: 20px;
  font-weight: 300;
  text-align: center;
  font-style: italic;
}

.notFound {
  color: white;
  font-size: 20px;
  font-weight: 300;
  text-align: center;
}

.sunTime {
  text-align: center;
  color:white;

}

.sunRise {
  font-size: 20px;
  font-weight: 400;
  padding: 10px;
}

.fa-sun-o{
  color:yellow;
  font-size:30px;
}

.sunSet {
  font-size: 20px;
  font-weight: 400;
  padding: 10px;
}

.fa-moon-o{
  color:blue;
  font-size:30px;
}

.weather-box {
  text-align: center;
}

.weather-box .temp {
  display: inline-block;
  padding: 10px 25px;
  color: white;
  font-size: 102px;
  font-weight: 900;

  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0px;

  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.weather-box .weather {
  color: white;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
</style>

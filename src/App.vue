<template>
  <div id="app">
    <main
      :class="{
        warm: isWarm,
        clouds: isCloud,
        clear: isClear,
        haze: isHaze,
        rain: isRain,
        cold: isCold,
      }"
    >
      <h1>Weather App</h1>
      <div class="search-box">
        <input
          v-model="query"
          @keypress="fetchWeather"
          type="text"
          id="searchUser"
          placeholder="Search your city..."
        />
      </div>
      <div class="weather-wrap" v-if="weather">
        <div class="location-box">
          <div class="location">
            <span class="city-color">{{ weather.name }}</span
            >,<span class="country-color">{{ weather.sys.country }}</span>
          </div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>
        <div class="weather-main-box">
          <div class="temp">{{ Math.round(weather.main.temp) }}°c</div>
          <div class="weather">{{ weather.weather[0].main }}</div>
          <div class="weather-simple-box">
            <div class="clouds">
              <p>Clouds All: {{ weather.clouds.all }}%</p>
            </div>
            <div class="humidity">
              <p>Relative Humidity: {{ weather.main.humidity }}%</p>
            </div>
            <div class="pressure">
              <p>Air Pressure: {{ weather.main.pressure }}°</p>
            </div>
            <div class="wind">
              <p>Wind: {{ weather.wind.speed }}km/h</p>
            </div>
          </div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  data() {
    return {
      api_key: "f064a082d05edbbddf41d7254c8176ed",
      url_base: "https://api.openweathermap.org/data/2.5/",
      query: "",
      weather: null,
      imageUrl: "",
    };
  },
  computed: {
    isWarm() {
      return this.weather ? this.weather.weather[0].main === "Warm" : null;
    },
    isCloud() {
      return this.weather ? this.weather.weather[0].main === "Clouds" : null;
    },
    isHaze() {
      return this.weather ? this.weather.weather[0].main === "Haze" : null;
    },
    isRain() {
      return this.weather ? this.weather.weather[0].main === "Rain" : null;
    },
    isClear() {
      return this.weather ? this.weather.weather[0].main === "Clear" : null;
    },
    isCold() {
      return this.weather ? this.weather.weather[0].main === "cold" : null;
    },
  },
  mounted() {
    fetch(`${this.url_base}weather?q=bogra&units=metric&APPID=${this.api_key}`)
      .then((res) => {
        return res.json();
      })
      .then(this.setResults);
  },
  methods: {
    fetchWeather(e) {
      if (e.key == "Enter") {
        fetch(
          `${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`
        )
          .then((res) => {
            // console.log(res.name);
            return res.json();
          })
          .then(this.setResults);
      }
    },
    setResults(results) {
      this.weather = results;
      this.imageUrl =
        "http://openweathermap.org/img/w/" +
        `${this.weather.cod != 404 ? this.weather.weather[0].icon : null}` +
        ".png";
      console.log(results);
    },
    dateBuilder() {
      let d = new Date();
      let months = [
        "January",
        "February",
        "March",
        "April",
        "May",
        "June",
        "July",
        "August",
        "September",
        "October",
        "November",
        "December",
      ];
      let days = [
        "Sunday",
        "Monday",
        "Tuesday",
        "Wednesday",
        "Thursday",
        "Friday",
        "Saturday",
      ];
      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();
      return `${day} ${date} ${month} ${year}`;
    },
  },
};
</script>

<style>
/* reset */
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

h1 {
  font-size: 3rem;
  text-align: center;
  margin-top: 10px;
  margin-bottom: 30px;
  font-weight: bold;
  color: rgb(114, 114, 114);
}

p {
  color: rgb(176, 176, 176);
  font-size: 1.3rem;
  margin-bottom: 10px;
}

img {
  width: 80px;
}
#app {
  font-family: "roboto", sans-serif;
}
.city-color {
  color: #52edcb;
}
.country-color {
  color: rgb(6, 69, 6);
}
#app main {
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
  min-height: 100vh;
  padding: 10px;
}

#app .search-box {
  max-width: 400px;
  margin-bottom: 15px;
  margin: auto;
}

/* filter */
#searchUser {
  background-image: url("https://www.freeiconspng.com/thumbs/search-icon-png/search-icon-png-2.png");
  background-size: 15px 15px;
  background-position: 20px 17px;
  background-repeat: no-repeat;
  width: 100%;
  appearance: none;
  border: none;
  outline: none;
  color: #313131;
  display: block;
  padding: 10px;
  font-size: 15px;
  padding: 16px 30px 16px 55px;
  border-radius: 100px;
  outline: none;
  background-color: rgba(255, 255, 255, 0.5);
  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
  transition: 0.5s;
}

#searchUser:focus {
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(0, 0, 0, 0.75);
  color: #fff;
}

#app .weather-wrap .location-box .location {
  color: #fff;
  font-size: 40px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
  margin-top: 2rem;
}

/* date */
.date {
  color: rgb(6, 69, 6);
  font-size: 25px;
  font-weight: 300;
  font-family: roboto sans-serif;
  font-weight: bold;
  text-align: center;
  margin-top: 10px;
}
.weather-main-box {
  text-align: center;
}
.weather-simple-box {
  max-width: 400px;
  margin: auto;
  padding: 10px;
  background: rgb(92, 109, 92);
  border-radius: 10px;
}
.temp {
  display: inline-block;
  padding: 10px 25px;
  color: #fff;
  font-weight: 900;
  font-size: 70px;
  text-shadow: 3px 6px rgb(6, 69, 6);
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 16px;
  margin: 10px 0px;
  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
.weather {
  color: #52edcb;
  font-size: 30px;
  font-weight: 700px;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  padding-bottom: 20px;
}
/* weather image */
.rain {
  background-image: url("assets/image/rain.jpg");
}
.warm {
  background-image: url("assets/image/warm-bg.jpg");
}
.cold {
  background-image: url("assets/image/cold.jpeg");
}
main {
  background-image: url("assets/image/cloudy.jpg");
}
.haze {
  background-image: url("assets/image/haze.jpg");
}
.clear {
  background-image: url("assets/image/clear.jpg");
}
</style>

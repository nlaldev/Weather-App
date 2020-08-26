<template>
  <div class="app">
    <main class="search">
      <input
        type="text"
        class="search__bar"
        placeholder="Search..."
        v-model="query"
        @keypress="getWeather"
      />
    </main>
    <div class="weather__wrapper">
      <div class="weather__location-wrapper">
        <span class="weather__location">Vancouver, BC</span>
        <span class="weather__date">Aug 26 2020</span>
      </div>
      <div class="weather__box">
        <span class="weather__weather">Sunny</span>
        <span class="weather__temperature">20 C</span>
      </div>
    </div>
  </div>
</template>

<script>
import Vue from "vue";
import axios from "axios";

Vue.use(axios);

export default {
  name: "App",
  data() {
    return {
      api_key: "0f4aa31f6383d13c329b4ca035e1b8e9",
      url: "https://api.openweathermap.org/data/2.5/",
      query: "",
      weather: {},
    };
  },
  methods: {
    getWeather(e) {
      if (e.key == "Enter") {
        axios
          .get(
            `${this.url}weather?q=${this.query}&units=metric&APPID=${this.api_key}`
          )
          .then((res) => {
            console.log(res);
            this.weather = res;
          })
          .catch((err) => console.error(err));
        console.log("fetching weather...");
      }
    },
  },
};
</script>

<style lang="scss">
@font-face {
  font-family: "helvetica-neue";
  src: url("./assets/helveticaneue/helveticaneue-thin.ttf");
  font-weight: normal;
}

* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
  font-family: "helvetica-neue";
  text-align: center;
}

.app {
  background-image: url("./assets/warm-bg.jpg");
  background-position: bottom;
  background-size: cover;
  height: 100vh;
  transition: 0.5s;
}

.search {
  background: silver;
  &__bar {
    box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
    background-color: rgba(255, 255, 255, 0.5);
    border-radius: 0px 16px 0px 16px;
    transition: 0.4s;
    width: 90%;
    padding: 15px;
    font-size: 20px;

    &:focus {
      box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
      background-color: rgba(255, 255, 255, 0.75);
      border-radius: 16px 0px 16px 0px;
    }
  }
}
</style>

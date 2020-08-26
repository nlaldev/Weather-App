<template>
  <div
    class="app"
    :class="
      typeof weather.main !== 'undefined' && weather.main.temp >= 15
        ? 'app-warm'
        : ''
    "
  >
    <main class="search">
      <input
        type="text"
        class="search__bar"
        placeholder="Search..."
        v-model="query"
        @keypress="getWeather"
      />

      <div class="weather" v-if="typeof weather.main != 'undefined'">
        <span class="weather__location"
          >{{ weather.name }}, {{ weather.sys.country }}</span
        >
        <span class="weather__date">{{ dateBuilder() }}</span>
        <div class="weather__wrapper">
          <span class="weather__stat">{{ weather.weather[0].main }}</span>
          <div class="weather__temperature">
            {{ Math.round(weather.main.temp) }}&deg;c
          </div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      url_base: "https://api.openweathermap.org/data/2.5/",
      api_key: "0f4aa31f6383d13c329b4ca035e1b8e9",
      query: "",
      weather: {},
    };
  },
  methods: {
    getWeather(e) {
      if (e.key == "Enter") {
        fetch(
          `${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`
        )
          .then((res) => {
            return res.json();
          })
          .then(this.setResults)
          .catch((err) => console.error(err));
      }
    },

    setResults(results) {
      this.weather = results;
      console.log(this.weather);
    },

    dateBuilder() {
      const d = new Date();
      const months = [
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
      const days = [
        "Sunday",
        "Monday",
        "Tuesday",
        "Wednesday",
        "Thursday",
        "Friday",
        "Saturday",
      ];
      const day = days[d.getDay()];
      const date = d.getDate();
      const month = months[d.getMonth()];
      const year = d.getFullYear();
      return `${day} ${date} ${month} ${year}`;
    },
  },
  // },
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
  background: linear-gradient(to bottom, #2193b0, #6dd5ed);
  background-position: bottom;
  background-size: cover;
  min-height: 100vh;
  transition: 0.5s;
}

.app-warm {
  background: linear-gradient(to bottom, #f2c94c, #f2994a);
  transition: 0.5s;
}

.search {
  // background: linear-gradient(
  //   to bottom,
  //   rgba(0, 0, 0, 0.25),
  //   rgba(0, 0, 0, 0.75)
  // );
  min-height: 100vh;
  padding: 25px;

  &__bar {
    box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
    background-color: rgba(255, 255, 255, 0.5);
    border-radius: 10px;
    transition: 0.4s;
    width: 90%;
    padding: 15px;
    font-size: 20px;
    outline: none;
    border: none;

    &:focus {
      box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
      background-color: rgba(255, 255, 255, 0.75);
      border-radius: 20px;
    }
  }
}

.weather {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  margin-top: 50px;

  &__wrapper {
    padding: 30px;
    background-color: rgba(255, 255, 255, 0.25);
    border-radius: 20px;
    color: #fafafa;
  }

  &__stat {
    font-size: 20px;
    font-style: italic;
  }

  &__temperature {
    font-size: 90px;
    text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  }
}
</style>

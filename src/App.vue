<template>
  <div
    class="app"
    :class="
      typeof weather.main !== 'undefined' && weather.main.temp >= 15
        ? 'app-warm'
        : ''
    "
  >
    <div class="search">
      <input
        type="text"
        class="search__bar"
        placeholder="Search..."
        v-model="query"
        @keypress="getWeather"
      />
    </div>

    <div class="weather" v-if="typeof weather.main != 'undefined'">
      <span class="weather__date">{{ dateBuilder() }}</span>
      <div class="weather__wrapper">
        <div class="weather__stat">{{ weather.weather[0].main }}</div>
        <div class="weather__temperature">
          {{ Math.round(weather.main.temp) }}&deg;c
        </div>
      </div>
      <span class="weather__location"
        >{{ weather.name }}, {{ weather.sys.country }}</span
      >
    </div>
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
      return `${day}, ${month} ${date} `;
    },
  },
  // },
};
</script>

<style lang="scss">
$tablet-breakpoint: 768px;
$desktop-breakpoint: 1200px;

@mixin tablet {
  @media (min-width: $tablet-breakpoint) {
    @content;
  }
}

@mixin desktop {
  @media (min-width: $desktop-breakpoint) {
    @content;
  }
}

@font-face {
  font-family: "helvetica-neue";
  src: url("./assets/helveticaneue/helveticaneue-thin.ttf");
  font-weight: thin;
}

@font-face {
  font-family: "helvetica-neue";
  src: url("./assets/helveticaneue/helveticaneue-light.ttf");
  font-weight: lighter;
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
  min-height: 100vh;
  padding: 25px;
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
}

.app-warm {
  background: linear-gradient(to bottom, #f2c94c, #f2994a);
  transition: 0.5s;
}

.search {
  margin-top: 20px;
  &__bar {
    box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
    background-color: rgba(255, 255, 255, 0.5);
    border-radius: 10px;
    transition: 0.5s;
    padding: 15px;
    width: 100%;
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
  margin-top: 10vh;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  align-items: center;
  color: #fafafa;
  &__wrapper {
    padding: 30px;
    margin: 25px 0 50px 0;
    background-color: rgba(255, 255, 255, 0.25);
    border-radius: 20px;
    transition: 0.5s;
  }

  &__date {
    font-size: 24px;
  }

  &__stat {
    font-size: 20px;
    font-style: italic;
  }

  &__temperature {
    font-size: 90px;
    font-weight: lighter;
    text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  }
}

@include tablet {
  .search {
    &__bar {
      width: 25%;
    }
  }

  .weather {
    margin-top: 20vh;
    &__wrapper {
      width: 75%;
      display: flex;
      justify-content: space-around;
      align-items: center;
    }

    &__date {
      font-size: 48px;
    }

    &__stat {
      width: 50%;
    }

    &__temperature {
      width: 50%;
      border-left: 5px solid #fafafa;
    }

    &__location {
      font-size: 24px;
    }
  }
}
</style>

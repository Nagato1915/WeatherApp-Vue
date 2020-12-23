<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 20 ? 'warm': ''">
    <main>
      <div class="search-box">
        <input 
        type="text" 
        class="search-bar"
        placeholder="Place..."
        v-model = "query"
        @keydown="fetchApi"
        >
      </div>
      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">{{weather.name}}, {{ weather.sys.country }}</div>
          <div class="date">{{ setDate() }}</div>
        </div>
        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp) }}℃</div>
          <div class="weather">{{ weather.weather[0].main }}</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>


export default {
  name: 'App',
  data() {
    return {
      api_key: 'e5b2a1c261509925f43d2cc83d0981c0',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: "",
      weather: {}
    }
  },
  methods: {
    fetchApi(e) {
      if (e.key == "Enter") {
      fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
      .then(res => {
        return res.json();
      }).then(this.setResults)
      }
    },
    setResults(res) {
      this.weather = res;
    },
    setDate() {
      const d = new Date();
      let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
      let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];

      const day = days[d.getDay()];
      const date = d.getDate();
      const month = months[d.getMonth()];
      const year = d.getFullYear();

      return `${day} ${date} ${month} ${year}`
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

    #app {
      background-image: url("./images/cold-bg.jpg");
      background-size: cover;
      background-position: center;
    }

    #app.warm {
       background-image: url("./images/warm-bg.jpg");
    }

    main {
      width: 100%;
      margin: 0 auto;
      text-align: center;
      padding: 5rem;
      min-height: 100vh;
      background: rgba(0, 0, 0, .25);
    }

    .search-box {
      width: 100%;
      max-width: 1024px;
      margin: 0 auto 3rem;  
    }

    .search-bar {
      width: 100%;
      outline: none;
      font-size: 1.1rem;
      padding: 1rem;
      border: none;
      border-radius: 0px 1rem 0px 1rem;
      background: none;
      background-color: rgba(255, 255, 255, .5);
      box-shadow: 0px 0px 8px rgba(0, 0, 0, .25);
      transition: all .2s
    }

    .search-bar:focus {
      border-radius: 1rem 0px 1rem 0px;
      background-color: rgba(255, 255, 255, .75);
      box-shadow: 0px 0px 16px rgba(0, 0, 0, .25);
    }

    .location-box {
      margin-bottom: 1rem;
    }

    .location {
      font-size: 1.8rem;
      color: #fff;
    }

    .date {
      font-size: 1.3rem;
      color:#fff;
    }

    .temp {
      display: inline-block;
      font-size: 4rem;
      background: rgba(255, 255, 255, .25);
      border-radius: 1rem;
      padding: 10px 25px;
      color: #fff;
      font-weight: 700;
    }

    .weather {
      color: #fff;
      font-size: 2rem;
      font-weight: 700;
    }


</style>

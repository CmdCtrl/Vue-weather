<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 65 ?
    'warm' : ''">
    <main>
      <div class= "search-box">
        <input type="text" class="search-bar"
        autocomplete="off" 
        placeholder="Enter city or ZIP code..."
        v-model="query" @keypress="fetchWeather">
      </div>
      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">{{weather.name}}, {{weather.sys.country}}</div>
          <div class="date">{{dateBuilder()}}</div>
        </div>

        <div class="weather-box">
          <div class="temp">{{Math.round(weather.main.temp)}}°F</div>
          <div class="weather">{{weather.weather[0].main}}</div>
          <div class="high-low">{{Math.round(weather.main.temp_min)}}°F / {{Math.round(weather.main.temp_max)}}°F</div>
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
        api_key: '7197cdcfe5dea9df4fe0111f39dc23bb',
        url_base: 'https://api.openweathermap.org/data/2.5/',
        query: '',
        weather: {}
      }
    },
    methods: {
      fetchWeather(e) {
        if(e.key == "Enter"){
          if(isNaN(this.query)){
            fetch(`${this.url_base}weather?q=${this.query}&units=imperial&APPID=${this.api_key}`)
            .then(res => {
              return res.json();
            }).then(this.setResults);
          }else{
            fetch(`${this.url_base}weather?zip=${this.query}&units=imperial&APPID=${this.api_key}`)
            .then(res => {
              return res.json();
            }).then(this.setResults);
          }
        }
      },
      setResults (results) {
        this.weather = results;
        this.query = "";
      },
      dateBuilder () {
        let d = new Date();
        let months = ["January", "February", "March", "April", "May", 
        "June", "July", "August", "September", "October", "November", "December"];
        let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", 
        "Friday", "Saturday"];

        let date = d.getDate();
        let day = days[d.getDay()];
        let month = months[d.getMonth()];
        let year = d.getFullYear();

        return `${day} ${month} ${date}, ${year}`;
      }
    }
  }
</script>

<style>
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body{
  font-family: tahoma, verdana, sans-serif;
}

#app{
  background-image: url('./assets/cold-bg.jpg');
  background-repeat: no-repeat;
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}

#app.warm {
  background-image: url('./assets/warm-bg.jpg');
}

main{
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.3), rgba(0, 0, 0, 0.6));
}

.search-box{
  width: 100%;
  max-width: 600px;
  margin-top: 10px;
  margin-bottom: 30px; 
  margin-left: auto;
  margin-right: auto;
}

.search-box .search-bar{
  display: block;
  width: 100%;
  padding: 15px;
  color: #313131;
  font-size: 20px;
  
  appearance: none;
  border: none;
  outline: none;
  background: none;

  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 0px 16px 0px 16px;
  transition: 0.4s;
}

.search-box .search-bar:focus {
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color:rgba(255, 255, 255, 0.75);
  border-radius: 16px 0px 16px 0px;
}
.location-box .location {
  color: #fff;
  font-size: 32px;
  font-weight: 300;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}

.location-box .date {
  color: #fff;
  font-size: 18px;
  font-weight: 300;
  font-style: italic;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}

.weather-box {
  text-align: center;
}

.weather-box .temp {
  display: inline-block;
  padding: 10px 25px;
  color: #fff;
  font-size: 88px;
  font-weight: 650;

  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0px;

  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.weather-box .weather {
  color: #fff;
  font-size: 30px;
  font-weight: 700;
  font-style: italic;
  text-shadow:rgba(0, 0, 0, 0.25);
}

.weather-box .high-low {
  color: #fff;
  font-size: 22px;
  font-weight: 700;
  font-style: italic;
  text-shadow:rgba(0, 0, 0, 0.25);
}
</style>

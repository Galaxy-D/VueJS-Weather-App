<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 20 ? 'warm':''">
    <main>
      <div class="search-box">
        <!-- v-model create a two way binding on a form input element or component -->
        <input 
        type="text" 
        class="search-bar" 
        placeholder="Enter the desired area to get its weather forecast"
        v-model="query"
        @keypress="fetchWeather"
        >
      </div>
      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">{{weather.name}}, {{weather.sys.country}}</div>
          <div class="date">{{dateBuilder()}}</div>
        </div>
      </div>
      <div class="weather-box" v-if="typeof weather.main != 'undefined'">
        <div class="temp">{{Math.round(weather.main.temp)}} °C</div>
        <div class="weather-state">{{weather.weather[0].main}}</div>
      </div>
      <div class="CityNotFound" v-else-if="typeof weather.main === 'undefined'">{{ErrMessage}}</div>
    </main>
  </div>
</template>

<script>

  export default {
    name: 'App',
    data() {
      return {
        api_key : '4151507d1e3421f1c85072e7ee88ad3e',
        //API base, when we make request this is what needs to go at the front of our request
        url_base: 'https://api.openweathermap.org/data/2.5/',
        //this allow us to bind the search box 
        query: '',
        //to store weather data or we get back once we make the request
        weather: {},
        //error message
        ErrMessage: '',
      }
    },
    methods: {
      fetchWeather(event) {
        if(event.key == "Enter") {
          //is a JavaScript API
          fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
          .then(response => { 
            if(!response.ok) {
              this.weather = {},
              this.ErrMessage = `Sorry can't find " ${this.query} " city - Error ${response.status}`;
              throw new Error(`HTTP error! status: ${response.status}`);
            }else{return response.json();}})
          .then(data => {this.weather = data})
          .catch(() => {return `${this.ErrMessage}`}) 
        }   
      },
      dateBuilder() {
        let date  = new Date();   
        return `${date.toLocaleString()}`;
      }
    },
  }

</script>

<style>

  *{
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
  }

  #app.warm {
    background-image: url('./assets/warm-bg.jpg');
  }

  main {
    /* Viewport Height (vh). This unit is based on the height of the viewport. A value of 1vh is equal to 1% of the viewport height. */
    min-height: 100vh;
    padding: 25px;
    background-image: linear-gradient(to bottom, rgba(0,0,0,0.25), rgba(0,0,0,0.75));
  }

  .search-box {
    width: 100%;
    margin-bottom: 35px;
  }

  .search-box .search-bar {
    display: block;
    width: 100%;
    padding: 15px;
    color: #313131;
    font-size: 20px;
    /* The appearance CSS property is used to display an element using platform-native styling, based on the operating system's theme.  */
    appearance: none;
    border: none;
    outline: none;
    background: none;
    background-color: rgba(255, 255, 255, 0.5);
    border-radius: 0px 16px;
    transition: 0.4s;
    box-shadow: 15px 15px 20px rgba(0, 0, 0, 1);
  }

  .search-box .search-bar:focus {
    box-shadow: 15px 15px 40px rgba(0, 0, 0, 1);
    background-color: rgba(255, 255, 255, 1);
    border-radius: 16px;
  }

  .location-box .location {
    color: #FFF;
    font-size: 32px;
    font-weight: 500;
    text-align: center;
    text-shadow: 1px 3px rgba(0,0,0,0.25);
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
    display: inline-block;
    padding: 10px 25px;
    color: #FFF;
    font-size: 102px;
    font-weight: 900px;
    text-shadow: 3px 6px rgba(0,0,0,0.25);
    background-color: rgba(255, 255, 255, 0.25);
    border-radius: 10px;
    margin: 30px 0px;
    box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  }

  .weather-box .weather-state {
    color: #FFF;
    font-size: 48px;
    font-weight: 700;
    font-style: italic;
  }

  .CityNotFound {
    color: #FFF;
    font-size: 48px;
    font-weight: 700;
    font-style: italic;
    white-space: nowrap;
    text-align: center;
  }

  @media screen and (max-width: 900px) {
    
    .CityNotFound {
      font-size: 20px;
      font-weight: 700;
    }
    .search-box input::placeholder {
      font-size: 16px;
      font-weight: 700;
    }
  }
  

</style>

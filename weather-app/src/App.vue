<template>
 <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp>16 ? 'warm':''">
  <main>
    <h1>Weather App</h1>
    <div class="search-box">
      <input type="text"  class="search-bar" placeholder="Search..." v-model="query" @keypress="fetchWeather" />
    </div>
    
    <div class="weather-wrap" v-if= "typeof weather.main != 'undefined' ">
      <div class="location-box">
        <div class="location">{{weather.name }}, {{ weather.sys.country}}</div>
        <div class="date">{{dateBuilder()}}</div>
      </div>
      <div class="weather-box">
        <div class="temp">{{Math.round(weather.main.temp)}}<sup>o</sup>C</div>
        <div class="weather">{{ weather.weather[0].main }}</div>
        <div class="description">{{weather.weather[0].description}}</div>
      </div>
    </div>
  </main>
 </div>
</template>

<script>

export default {
  name: 'App',
  components: {
  
  },
  data(){
    return{
      api_key:'8f571b39b2486f65093a49aea0b3d730',
      url_base:'https://api.openweathermap.org/data/2.5/',
      query:'',
      weather:{}
    }
  },
  methods:{
      async fetchWeather(e){
        if(e.key=="Enter"){
    
         const response= await fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`,
         {
          method:'GET'
         });
          this.weather=await response.json();
      
        }
      },
      setResults(results){
        this.weather=results;
      },
      dateBuilder(){
        let d=new Date();
        let months=['January','Febrauary','March',"April","May","June","July","Augest","September","October","November","December"];
        let days=['Sunday','Monday','Tuesday','wednesday','Thursday','Friday',"Saturday"];

        let day=days[d.getDay()];
        let date=d.getDate();
        let month=months[d.getMonth()];
        let year=d.getFullYear();

        return`${day}, ${date} ${month} ${year}`;
      }
  }
}
</script>

<style>
  *{
    margin:0;
    padding: 0;
    box-sizing: border-box;
  }

  body{
    font-family: 'montserrat' sans-serif;
  }

  h1{
    margin:0px 0px 10px 0px;
    text-align: center;
    color: purple;
    font-size: 50px;
    font-weight: bolder;
  }

  #app{
    background-image: url('./assets/cold-bg.jpg');
    background-size: cover;
    background-position: bottom;
    transition: 0.4s;
  }

  #app.warm{
     background-image: url('./assets/warm-bg.jpg');
  }

  main{
    min-height: 100vh;
    padding: 25px;

    background-image: linear-gradient(to bottom,rgba(0,0,0,0.25),rgba(0,0,0,0.75));

  }

  .search-box{
    width: 100%;
    margin-bottom: 30px;
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

    box-shadow: 0px 0px 16px rgba(0,0,0,0.25);
    background-color: rgba(255,255,255,0.5);
    border-radius: 0px 16px 0px 16px;
    transition: 0.4s;
  }

  .search-bar .search-box:focus{
    box-shadow: 0px 0px 16px rgba(0,0,0,0.25);
    background-color: rgba(255,255,255,0.75);
    border-radius: 16px 0px 16px 0px;
  }

  .location-box .location {
    color: #fff;
    font-size: 32px;
    font-weight: 500;
    font-style: italic;
    text-align:center;
  }

  .location-box .date {
    color: #fff;
    font-size: 32px;
    font-weight: 500;
    font-style: italic;
    text-align:center;
  }

  .weather-box{
    text-align: center;
  }

  .weather-box .temp{
    display: inline-block;
    padding: 10px 25px;
    color: #fff;
    font-size: 102px;
    font-weight: 900;

    text-shadow: 3px 6px rgba(0,0,0,0.25);
    background-color: rgba(255,255,255,0.25);
    border-radius: 16px;
    margin: 30px 0px;

    box-shadow: 3px 6px rgba(0,0,0,0.25);
  }

  .weather-box .weather{
    color: #fff;
    font-size: 48px;
    font-weight: 700;
    font-style: italic;
    text-shadow: 33px 6px rgba(0,0,0,0.25);
  }

   .weather-box .description{
    color: #fff;
    font-size: 20px;
    font-weight: 300;
    font-style: italic;
    text-shadow: 20px 4px rgba(0,0,0,0.25);
  }

</style>

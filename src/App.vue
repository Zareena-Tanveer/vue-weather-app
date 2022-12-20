<template>
  <div id="app" :class="(typeof weather.main != 'undefined') && weather.main.temp > 16 ?'warm':''">
    <main>
      <!-- <h1>{{msg}}</h1> -->
      <div class="search-box">
        <input 
          type="text"
          class="search-bar" 
          placeholder="Search Weather..." 
          v-model="query"
          @keyup.enter="fetchWeather"
        >
      </div>
      <div class="weather-wrap" v-if="(typeof weather.main != 'undefined')">
        <div class="location-box">
          <div class="location">
            <!-- <img v-bind:src="icon"/> -->
            {{weather.name}}, {{weather.sys.country}}
          </div>
          <div class="date"> {{currentDate}}
          </div>
        </div>
        <div class="weather-box">
          <div class="temp">
            
            <!-- <img :src="icon" alt=""> -->
            {{Math.round(weather.main.temp)}} &#x2103;
          </div>
          <div class="weather">
            {{weather.weather[0].main}}
            
          </div>
          <div class="desc">
            Feel Like {{Math.round(weather.main.feels_like)}} &#x2103; - {{weather.weather[0].description}}
          </div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
  export default {
    name:'app',
    data(){
      return {
        msg:'Weather App',
        api_key: '74342f1118adbecd7b1317dd1feac122',
        url_base: 'https://api.openweathermap.org/data/2.5/',
        query:'',
        weather:{}, //object,
        currentDate:'',
        icon:''
      }
    },
    
    methods: {
      fetchWeather(){
        if (this.query !=='') {
          fetch(`${this.url_base}weather?q=${this.query}&units=metric&appid=${this.api_key}`)
            .then(res => res.json())
            .then(this.setResults)
            .catch(err => console.error(err));
        }
        else{
          alert('Please Enter City Name...');
        }
      },
      setResults(results){
        this.weather = results;
        this.setIconSrc();
        this.getDate();

      },
      setIconSrc(){
        this.icon = "http://openweathermap.org/img/w/"+this.weather.weather[0].icon+".png";
      },
      getDate(){
        let newDate = new Date(this.weather.dt);
        const weekday = ["Sunday","Monday","Tuesday","Wednesday","Thursday","Friday","Saturday"];
        const month = ["January","February","March","April","May","June","July","August","September","October","November","December"];
        let day = weekday[newDate.getDay()];
        let name = month[newDate.getMonth()];
        let date = newDate.getDate();
        let year = newDate.getFullYear();
        this.currentDate =day+', '+date+' '+name+' '+ year;
      }
    }
  }
</script>

<style scoped>
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body{
  font-family: Georgia, 'Times New Roman', Times, serif;
  
}

#app{
  background: url('./assets/cold-bg.jpg') no-repeat bottom;
  background-size: cover;
  transition: 0.4s;
}
#app.warm{
  background: url('./assets/warm-bg.jpg') no-repeat bottom;
  background-size: cover;
  transition: 0.4s;
}
main{
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(to bottom, rgba(0,0,0,0.25), rgba(0,0,0,0.75));
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
  font-size: 15px;
  appearance: none;
  border: none;
  outline: none;
  background: none;
  box-shadow: 0px 0px 8px rgba(0,0,0,0.25);
  background-color: rgba(255,255,255,0.5);
  border-radius: 0px 16px;
  transition: 0.4s;
}

.search-box .search-bar:focus{
  box-shadow: 0px 0px 16px rgba(0,0,0,0.25);
  background-color: rgba(255,255,255,0.75);
  border-radius: 16px 0px;
  
}

.location-box .location{
  color: #FFF;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0,0,0,0.25);
}

.location-box .date{
  color: #FFF;
  font-size: 16px;
  font-weight: 300;
  font-style: italic;
  text-align: center;
  text-shadow: 1px 3px rgba(0,0,0,0.25);
}
.weather-box{
  text-align: center;

}
.weather-box .temp{
  display: inline-block;
  padding: 10px 25px;
  color: #FFF;
  font-size: 102px;
  font-weight: 900;
  text-shadow: 3px 6px rgba(0,0,0,0.25);
  background-color: rgba(255,255,255,0.25);
  border-radius: 16px;
  margin: 30px 0px;
  box-shadow: 3px 6px rgba(0,0,0,0.25);
}

.weather-box .weather{
  color: #FFF;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0,0,0,0.25);
}
.weather-box .desc{
  color: #FFF;
  font-size: 20px;
  font-weight: 300;
  font-style: italic;
  text-shadow: 3px 6px rgba(0,0,0,0.25);
}
</style>

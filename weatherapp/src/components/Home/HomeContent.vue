<template>
<!-- <SearchBar city="city" che=checkWeather()/> -->
<div class="container">
    <div class="name">WeatherApp</div>
        <div class="search">
            <input v-model="city" type="text" placeholder="enter your city name" spellcheck="false" 
            @keyup.enter="checkWeather()" required>
            <button @click="checkWeather()"><img src="@/images/search.png"></button>
         <button @click="callApisSequentially()"><img src="@/images/location.png"></button>
        </div>
        <div  class="error-message">
            <p v-if="errorMessage">{{ errorMessage }}</p>
            <p v-else-if="city" class="error-message">{{}}</p>

        </div>
                

        <div class="weather" v-if="typeof weatherData.main !='undefined'">
            <img class="weather-icon1" src="@/images/clear_sky.png"/>
            <img :src="getWeatherIconUrl(weatherData.weather[0].icon)" class="weather-icon" alt="">  
       <div>

        <div>
            <h2 class="city">weather for {{weatherData.name}}</h2>
        </div>

        <h1 v-bind:value="city" class="temp" >
              <p v-if="city">{{Math.round(temparature)}}&#8451;</p>              </h1>
        </div>
            <div class="details">
                <div class="col">
                    <div>
                    <img src="@/images/humidity.png"/>
                    
                        <p v-bind:value="humidity" class="humidity">{{humidity}}&#37;</p>
                        <p>Humidity</p>
                    </div>
                    </div>
                <div class="col">
                    <div>
                    <img src="@/images/precipitation.png"/>
                    
                        <p v-bind:value="Precipitation" class="humidity">{{Precipitation}}&#37;</p>
                        <p>Precipitation</p>
                    </div>
                    </div>
                    <div class="col">
                        <div>
                        <img src="@/images/wind.png"/>
                            <p v-bind="wind" class="wind">{{wind}} m/s</p>
                            <p>WindSpeed</p>
                        </div>
                </div>
            </div>
        </div>
        <p style="color:black">Developed &#10084; with India &#64;Venu Katike</p>
</div>
</template>

<script>
//import SearchBar from './SearchBar.vue'
export default{
  components: {  },
  name: 'HomePage',
  data() {
    return {
      humidity:0,
      wind:0,
      city:"",
      apiKey: "e6ad137c876389eb7e5c3a94456a3048",
      apiUrl: "https://api.openweathermap.org/data/2.5/weather?",
      weatherData:{},
      errorMessage:"",
      temparature:Number,
      Precipitation:0,
    }
  },
  
  
methods: {   
     async getCity() {
      try {
        if ('geolocation' in navigator) {
          const position = await this.getCurrentPositionAsync();
          if (position) {
            const { latitude, longitude } = position.coords;
            const response = await fetch(`https://nominatim.openstreetmap.org/reverse?format=json&lat=${latitude}&lon=${longitude}`);
            const data = await response.json();
            this.city = data.address.city || 'City not found';
          } else {
            this.city = 'Location not available';
          }
        } else {
          this.city = 'Geolocation not supported';
        }
      } catch (error) {
        console.error('Error fetching city data:', error);
        this.city = 'City not found';
      }
    },
    getCurrentPositionAsync() {
      return new Promise((resolve, reject) => {
        navigator.geolocation.getCurrentPosition(resolve, reject);
      });
    },
    async checkWeather() {
        try
        {
            const city = this.city;
            const apiKey1 = this.apiKey
            const response = await fetch(`${this.apiUrl}q=${city}&appid=${apiKey1}`);
            //this.weatherData = await response.json();
     if (response.status === 404) {
      // Handle the 404 error here
      console.error('HTTP Error: 404 Not Found');
      this.errorMessage = "City not found"; // Set an error message for the user
    } else if (!response.ok) {
      // Handle other non-success HTTP status codes
      console.error(`HTTP Error: ${response.status} - ${response.statusText}`);
      this.errorMessage = "An error occurred while fetching weather data."; // Set an error message for the user
    } else {
      // If the status is 200 (OK), proceed with processing the response
      this.weatherData = await response.json();
      this.temparature= (this.weatherData.main.temp-273.5);
      try{
      if (this.WeatherData.rain != null )
            {
                const precipitationValue = this.WeatherData.rain._1h;
                const precipitationPercentage = (precipitationValue / 100) * 100;
                this.Precipitation = Math.Round(precipitationPercentage, 2);
            }
            }

            catch{
            this.Precipitation = 0;
            }

    }
  } catch (error) {
    console.error('Error getting weather data:', error);
    this.weatherData = {}; // Reset the weatherData on error
    this.errorMessage = "An error occurred while fetching weather data."; // Set an error message for the user
  }
    
        if(typeof this.weatherData.main !='undefined')
        {
        this.wind=this.weatherData.wind.speed;
        this.humidity=this.weatherData.main.humidity;
        }
      },
      getWeatherIconUrl(iconCode) 
      {
      return `https://openweathermap.org/img/w/${iconCode}.png`;
      },
    async callApisSequentially() {
      await this.getCity();
      await this.checkWeather();
      await this.getWeatherIconUrl();
      // Here, both API calls have completed sequentially
    },
  
},
    mounted()
    {
        this.callApisSequentially();
    }
}
</Script>


<style >
    .name {
    padding: 5px;
    font-size: xx-large;
    font-family: none;
    color: black;
}
.error-message {
  color: red;
  font-weight: bold;
  margin-top: 10px;
}
.container{
    color: #fff;
    border-radius: 20px;
    text-align: center;
    margin: 10px 100px;

}
.search{
    width: 100%;
    display: flex;
    align-items: center;
    justify-content: space-between;
    
}
.search input{
    border: 0;
    outline: 0;
    background: #ebfffc;
    color: #555;
    padding: 10px 25px;
    height: 50px;
    border-radius: 30px;
    flex: 1;
    margin-right: 15px;
    font-size: 18px;

}

.search button{
    border: 0;
    outline: 0;
    background: #ebfffc;
    border-radius: 50%;
    width: 60px;
    height: 60px;
    cursor: pointer;

}

.search button img{
        width: 16px;
}

.weather-icon1 {
    height: 240px;
}
.weather-icon {
    width: 240px;
    margin-left: -250px;
}
.weather h1 {
    font-size:60px;
    font-weight:500;
}
.weather h2 {
    font-size:45px;
    font-weight:400;
    margin-top:-10px;
}

.details{
    display: flex;
    align-items: center;
    justify-content: space-between;
    margin-top: 0px;
    font-size: 30px;

}
.col{
    display:flex;
    align-items:center;
    text-align:center;
 
}
.col img{
    width:100px;
    margin-right:10px;
}

@media (min-width: 200px) and (max-width: 700px) {

    .name {
    padding: 5px;
    font-size: xx-large;
    font-family: none;
    color: black;
}
  .container {
    color: #fff;
    border-radius: 20px;
    text-align: center;
    margin: 0px;

}
  .card{
    width: 100%;
    max-width: 1000px;
    color: #fff;
    border-radius: 20px;
    padding: 40px 35px;
    text-align: center;
}
.search {
    width: 100%;
    display: flex;
    align-items: center;
    justify-content: space-between;
    background: #ebfffc;
    border-radius: 30px;
}

.search input {
    border: 0;
    outline: 0;
    background: #ebfffc;
    color: #555;
    padding: 10px 25px;
    height: 50px;
    border-radius: 20px;
    flex: 1;
    margin-right: 6px;
    font-size: 18px;
}

.search button{
    border: 0;
    outline: 0;
    background: #ebfffc;
    border-radius: 50%;
    width: 60px;
    height: 60px;
    cursor: pointer;

}

.search button img{
        width: 30px;
}
.weather-icon1 {
    width: 130px;
    height: 100px;
    margin-left: 0px;
}
.weather-icon {
    width: 100px;
    margin-left: -110px;
}
.weather h1 {
    font-size:80px;
    font-weight:500;
}
.weather h2 {
    font-size:45px;
    font-weight:400;
    margin-top:-10px;
}

.details{
    display:flex;
    align-items:center;
    justify-content: space-between;
    margin: 25px;

}
.col{
    display:flex;
    align-items:center;
    text-align:center;
    font-size: 20px;
    font-family: system-ui;
 
}
.col img{
    width: 60px;
    height: 60px;
    margin-right: 0px;
}
}
</style>

<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 16 ? 'warm' : ''">
      <main>
          <div class="pesquisa_box">
            <input v-model="query" v-on:keyup.enter="fetchWeather" type="text" class="pesquisa-bar" placeholder="Procurar por..">
            <button v-on:click="fetchWeather">Buscar</button>

          </div>
          <div class="weather-wrap" v-if="teste">
            <div class="location-box">
              <div class="location">{{weather.name}}, {{weather.sys.country}}</div>
              <div class="date">{{dataBuilder()}}</div>
            </div>

            <div class="weather-box">
              <div class="temp">{{Math.round(weather.main.temp)}}°c</div>
              <div class="weather">{{alter(weather.weather[0].main)}}</div>
            </div>
          </div>
      </main>
      <Rodape text="&copy Clima ao redor do mundo"/>
  </div>
</template>

<script>
import Rodape from './components/Rodape';

export default {
  name: 'App',
  components: {
    Rodape
  },

  data(){
    return {
      api_key: '6159f5ce88c1d265558914d5460946d1',
      url_base:"https://api.openweathermap.org/data/2.5/",
      query:'São Paulo',
      weather: {},
      teste:false
    }
  },
  created(){
    this.fetchWeather();
  },

  methods:{
    fetchWeather(){
        this.teste = true;
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`).then(resp =>{
          return resp.json();
        }).then(this.setResults);
      
    },
    setResults (results){
      this.weather = results;
    },
    dataBuilder (){
      let d = new Date();
      let months = ["Janeiro","Fevereiro","Março","Abril","Maio","Junho","Julho",
      "Agosto","Setembro","Outubro","Novembro","Dezembro"];
      let days = ["Domingo","Segunda","Terça-Feira","Quarta-Feira","Quinta-Feira","Sexta-Feira","Sábado"];

      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();

      return `${day} - ${date} de ${month} - ${year}`;
    },
    alter(valor){
      if(valor == 'Clear'){
        valor = 'Céu limpo';
      }else if(valor == 'Clouds'){
        valor = 'Nublado'
      }
      return valor
    }
  }
}
</script>

<style>
*{
  border:0;
  margin:0;
  box-sizing: border-box;
}
#app {
  font-family:'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  background-image: url('./assets/cold-bg.jpg');
  background-size: 100% 100%;
  background-position: bottom;
  transition: 0.5s;
  min-height: 100vh;
  max-height: 100vh;
}
#app.warm{
  background-image: url('./assets/warm-bg.jpg');
}
main{
  min-height: 100vh;
  padding: 25px 50px;
  background-image: linear-gradient(to bottom, rgba(0,0,0,0.3), rgba(0,0,0,0.9));
  margin:0 auto;
}
.pesquisa-box{
 text-align: center;
  margin-bottom: 30px;
  display: flex;
  justify-content: space-between;
  text-align: center;
  width: 100%;
}
.pesquisa-bar{
  display: inline-block;  
  width: 87%;
  padding: 15px;
  color:#313131;
  font-size: 15px;
  border:0;
  box-shadow: 0px 0px 8px rgba(0,0,0,0.2);
  background-color: rgba(255,255,255,0.9);
  border-radius: 0px 16px 0px 16px;
  transition: 0.4s;
  outline: none;
}
button{
  color:#fff;
  font-weight: bold;
  font-size: 16px;
  margin-left:2%;
  width: 10%;
  background-color: #106afa;
  padding:12px 8px;
  text-align: center;
  border-radius: 7px;
  cursor:pointer;
  box-shadow: 3px 4px 5px 2px rgba(0,0,0,0.1);
  border:1px solid rgba(255,255,255,0.2);
  transition: 0.7s cubic-bezier(0.075, 0.82, 0.165, 1);
}
button:hover{
  filter: brightness(1.4);
}
.pesquisa-bar:focus{
  box-shadow: 0px 0px 16px rgba(0,0,0,0.2);
  background-color: rgba(255,255,255,1);
  border-radius: 16px 0px 16px 0px;
  font-size: 17px;
}

.location-box .location{
  color:#fff;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0,0,0,0.2);
}
.location-box .date{
  color:#fff;
  font-size: 17px;
  font-weight:lighter;
  font-style: italic;
  text-align: center;
}

.weather-box{
  text-align: center;
}
.weather-box .temp{
  display: inline-block;
  padding: 1px 35px;
  color:#fff;
  font-weight: 900;
  font-size: 90px;
  text-shadow: 3px 6px rgba(0,0,0,0.2);
  background-color: rgba(255,255,255,0.3);
  margin:15px 0;
  border-radius: 16px;
  box-shadow: 3px 4px rgba(0,0,0,0.2);
}
.weather-box .weather{
  color:#fff;
  font-size: 35px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0,0,0,0.3);
}
.weather-wrap{
  padding:20px 0;
}
.location-box{
  padding: 20px 0;
}
.weather{
  padding:10px 0;
}

@media (max-width:800px){
  button{
    display: block;
    margin:8px auto;
    padding: 10px 40px;
    text-align: center;
    font-size: 16px;
    width: 140px;
    position:relative;
    top:10px;
  }
  .pesquisa-bar{
    width: 100%;
  }
}
</style>

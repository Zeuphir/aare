<template>
  <div id="app" style="width: 100%; height: 100%;">
  <link href="https://fonts.googleapis.com/icon?family=Material+Icons"
            rel="stylesheet">
  <div :class="above ? 'redBody':'blueBody'" class="row" >
  <div class="snowflakes" aria-hidden="true" v-if="(!above) && snow_rain">
    <div class="snowflake">
                  ❅
              </div>
              <div class="snowflake">
                  ❅
              </div>
              <div class="snowflake">
                  ❆
              </div>
              <div class="snowflake">
                  ❄
              </div>
              <div class="snowflake">
                  ❅
              </div>
              <div class="snowflake">
                  ❆
              </div>
              <div class="snowflake">
                  ❄
              </div>
              <div class="snowflake">
                  ❅
              </div>
              <div class="snowflake">
                  ❆
              </div>
              <div class="snowflake">
                  ❄
              </div>
          </div>
          <div class="raindroplets" aria-hidden="true" v-if="above && snow_rain">
                      <div class="droplet">💧</div>
                      <div class="droplet">💧</div>
                      <div class="droplet">💧</div>
                      <div class="droplet">💧</div>
                      <div class="droplet">💧</div>
                      <div class="droplet">💧</div>
                      <div class="droplet">💧</div>
                      <div class="droplet">💧</div>
                      <div class="droplet">💧</div>
                      <div class="droplet">💧</div>
                      <div class="droplet">💧</div>
          </div>
          <div class="column">
  <h1 style="color: white; text-align:left; padding-left: 5vw; padding-top: 5vh">AARE TEMPERATUR</h1>
              <p style="font-size:22px; color: white; text-align:left; padding-left: 5vw; padding-top: 5vh">Lufttemperatur: {{air_temp}} </p>
              <p style="font-size:22px; color: white; text-align:left; padding-left: 5vw; padding-top: 5vh">Volumenstrom: {{flow}} m<sup>3</sup>/s</p>
              <p style="font-size:22px; color: white; text-align:left; padding-left: 5vw; padding-top: 5vh">Niederschlagshöhe: {{rainmm}} mm</p>
              <div style="padding-top: 5vh; text-align:left; padding-left: 5vw;">
    <input type="number" v-model="manual" id="temp_man" name="temp_man"
                   min="-30" max="56" style="padding:3px; height:24px; width:10vw"/>
            <div  class="material-icons" :class="above ? 'example_a':'example_b'" v-on:click="clear()" style = "position:relative; top:8px">highlight_off</div>
            <div class="material-icons" :class="above ? 'example_a':'example_b'" v-on:click="refresh()" style = "position:relative; top:8px">replay</div>
            </div>
            </div>
            <div class="column" style="float: right">
            <Thermometer :temp='temperature' :fl='flow' :airTemp='air_temp' :rainm='rainmm' :man='manual' />
            </div>
  </div>
  </div>
</template>

<script>
import Thermometer from './components/thermo.vue'
import axios from "axios"

export default {
  name: 'App',
  components: {
    Thermometer
  },
  data: function () {
    return {
        temperature: 10,
        flow: 0,
        air_temp: 0,
        rainmm: 0,
        snow_rain: false,
        manual: null,
        abov: true
    }
  },
  computed:{
    above() {
        if (this.manual) {
                        if (2 * this.manual + 67 < 67.0) {
                            return false;
                        } else {
                            return true;
                        }
        } else {
                        if (2 * this.temperature + 67 < 67.0) {
                            return false
                        } else {
                            return true
                        }
        }
    }
  },
  methods: {
    clear: function () {
        {
            this.manual = null;
        }
    },
    refresh: function () {
        axios.get('https://aareguru.existenz.ch/v2018/current').then(response => {
            this.temperature = response.data.aare.temperature;
            this.flow = response.data.aare.flow;
            this.air_temp = response.data.weather.current.tt.toString() + '°';
            this.rainmm = response.data.weather.current.rrreal;
            if (this.rainmm > 0) {
                this.snow_rain = true;
            }
        });
    },

  },
  mounted () {
          axios.get('https://aareguru.existenz.ch/v2018/current').then(response => {
                      this.temperature = response.data.aare.temperature;
                      this.flow = response.data.aare.flow;
                      this.air_temp = response.data.weather.current.tt.toString() + '°';
                      this.rainmm = response.data.weather.current.rrreal;
                      if (this.rainmm > 0) {
                          this.snow_rain = true;
                      }
                  })
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

.row {
    display: flex;
}

.column {
    flex: 50%;
}

.example_a {
     color: #fff !important;
     background: #FFBDBD;
     text-transform: uppercase;
     text-decoration: none;
     padding: 5px;
     border-radius: 5px;
     display: inline-block;
     border: none;
     transition: all 0.4s ease 0s;
 }

.example_a:hover {
    background: #FF0000;;
    letter-spacing: 1px;
    -webkit-box-shadow: 0px 5px 40px -10px rgba(0,0,0,0.57);
    -moz-box-shadow: 0px 5px 40px -10px rgba(0,0,0,0.57);
    box-shadow: 5px 40px -10px rgba(0,0,0,0.57);
    transition: all 0.4s ease 0s;
}

.example_b {
    color: #fff !important;
    background: #BDBDFF;
    text-transform: uppercase;
    text-decoration: none;
    padding: 5px;
    border-radius: 5px;
    display: inline-block;
    border: none;
    transition: all 0.4s ease 0s;
}

.example_b:hover {
    background: #0000FF;;
    letter-spacing: 1px;
    -webkit-box-shadow: 0px 5px 40px -10px rgba(0,0,0,0.57);
    -moz-box-shadow: 0px 5px 40px -10px rgba(0,0,0,0.57);
    box-shadow: 5px 40px -10px rgba(0,0,0,0.57);
    transition: all 0.4s ease 0s;
}

.redBody {
     background-color: #ff9d9d;
}

.blueBody {
    background-color: #9595FF;
}

.snowflake {
    color: #fff;
    font-size: 1em;
    font-family: Arial;
    text-shadow: 0 0 1px #000;
}

@-webkit-keyframes snowflakes-fall{0%{top:-10%}100%{top:100%}}
@-webkit-keyframes snowflakes-shake{0%{-webkit-transform:translateX(0px);transform:translateX(0px)}50%{-webkit-transform:translateX(80px);transform:translateX(80px)}100%{-webkit-transform:translateX(0px);transform:translateX(0px)}}
@keyframes snowflakes-fall{0%{top:-10%}100%{top:100%}}
@keyframes snowflakes-shake{0%{transform:translateX(0px)}50%{transform:translateX(80px)}100%{transform:translateX(0px)}}.snowflake{position:fixed;top:-10%;z-index:9999;-webkit-user-select:none;-moz-user-select:none;-ms-user-select:none;user-select:none;cursor:default;-webkit-animation-name:snowflakes-fall,snowflakes-shake;-webkit-animation-duration:10s,3s;-webkit-animation-timing-function:linear,ease-in-out;-webkit-animation-iteration-count:infinite,infinite;-webkit-animation-play-state:running,running;animation-name:snowflakes-fall,snowflakes-shake;animation-duration:10s,3s;animation-timing-function:linear,ease-in-out;animation-iteration-count:infinite,infinite;animation-play-state:running,running}.snowflake:nth-of-type(0){left:1%;-webkit-animation-delay:0s,0s;animation-delay:0s,0s}.snowflake:nth-of-type(1){left:10%;-webkit-animation-delay:1s,1s;animation-delay:1s,1s}.snowflake:nth-of-type(2){left:20%;-webkit-animation-delay:6s,.5s;animation-delay:6s,.5s}.snowflake:nth-of-type(3){left:30%;-webkit-animation-delay:4s,2s;animation-delay:4s,2s}.snowflake:nth-of-type(4){left:40%;-webkit-animation-delay:2s,2s;animation-delay:2s,2s}.snowflake:nth-of-type(5){left:50%;-webkit-animation-delay:8s,3s;animation-delay:8s,3s}.snowflake:nth-of-type(6){left:60%;-webkit-animation-delay:6s,2s;animation-delay:6s,2s}.snowflake:nth-of-type(7){left:70%;-webkit-animation-delay:2.5s,1s;animation-delay:2.5s,1s}.snowflake:nth-of-type(8){left:80%;-webkit-animation-delay:1s,0s;animation-delay:1s,0s}.snowflake:nth-of-type(9){left:90%;-webkit-animation-delay:3s,1.5s;animation-delay:3s,1.5s}
.droplet {
    color: #0000FF;
    font-size: 1em;
    font-family: Arial;
    text-shadow: 0 0 1px #000;
}


@keyframes droplet-fall{0%{top:-10%}100%{top:100%}}
@keyframes shake{0%{transform:translateX(0px)}50%{transform:translateX(80px)}100%{transform:translateX(0px)}}.droplet{position:fixed;top:-10%;z-index:9999;-webkit-user-select:none;-moz-user-select:none;-ms-user-select:none;user-select:none;cursor:default;-webkit-animation-name:droplet-fall;-webkit-animation-duration:5s,3s;-webkit-animation-timing-function:linear,ease-in-out;-webkit-animation-iteration-count:infinite,infinite;-webkit-animation-play-state:running,running;animation-name:droplet-fall;animation-duration:5s,3s;animation-timing-function:linear,ease-in-out;animation-iteration-count:infinite,infinite;animation-play-state:running,running}.droplet:nth-of-type(0){left:1%;-webkit-animation-delay:0s,0s;animation-delay:0s,0s}.droplet:nth-of-type(1){left:10%;-webkit-animation-delay:1s,1s;animation-delay:1s,1s}.droplet:nth-of-type(2){left:20%;-webkit-animation-delay:6s,.5s;animation-delay:6s,.5s}.droplet:nth-of-type(3){left:30%;-webkit-animation-delay:4s,2s;animation-delay:4s,2s}.droplet:nth-of-type(4){left:40%;-webkit-animation-delay:2s,2s;animation-delay:2s,2s}.droplet:nth-of-type(5){left:50%;-webkit-animation-delay:8s,3s;animation-delay:8s,3s}.droplet:nth-of-type(6){left:60%;-webkit-animation-delay:6s,2s;animation-delay:6s,2s}.droplet:nth-of-type(7){left:70%;-webkit-animation-delay:2.5s,1s;animation-delay:2.5s,1s}.droplet:nth-of-type(8){left:80%;-webkit-animation-delay:1s,0s;animation-delay:1s,0s}.droplet:nth-of-type(10){left:25%;-webkit-animation-delay:3s,1.5s;animation-delay:3s,1.5s}.droplet:nth-of-type(9){left:90%;-webkit-animation-delay:3s,1.5s;animation-delay:3s,1.5s}

</style>

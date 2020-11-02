<template>
  <div id="app">
    <HelloWorld :temp='temp' msg="Welcome to Your Vue.js App" :datum='datum'/>
    <Thermometer/>
    <input type="number" v-model="manual" id="temp_man" name="temp_man"
                   min="-30" max="56"/>
            <div  class="material-icons" :class="above ? 'example_a':'example_b'" v-on:click="clear()">highlight_off</div>
            <div class="material-icons" :class="above ? 'example_a':'example_b'" v-on:click="refresh()">replay</div>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'
import Thermometer from './components/thermo.vue'
import axios from "axios"

export default {
  name: 'App',
  components: {
    HelloWorld,
    Thermometer
  },
  data: function () {
    return {
        test: 0,
        temp: 10,
        datum: null,
        manual: null
    }
  },
  mounted () {
          axios
              .get('https://www.wiewarm.ch:443/api/v1/temperature.json/17')
              .then((response) => {
                  this.temp = parseFloat(response.data['52']['temp'])
                  this.datum = (new Date(Date.parse(response.data['52']['date']))).toLocaleDateString('de-CH',{ day: '2-digit', month: '2-digit',year: 'numeric', hour: '2-digit', minute: '2-digit'  })
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
  margin-top: 60px;
}
</style>

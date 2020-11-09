<template>
    <div class="thermo">
    <svg id="thermo" width="calc(100vh / 5 * 2)" height="100vh"
                     style="fill: none; stroke: white; stroke-linejoin: round; stroke-linecap: round;"
                     viewBox="0 0 100 300">
    <defs>
    <filter id="f1" x="0" y="0">
        <feGaussianBlur in="SourceGraphic" stdDeviation="25" />
    </filter>
    </defs>

    <linearGradient id="grad1" x1="0%" y1="0%" x2="70%" y2="0%" >
    <stop offset="0%" style="stop-opacity:0.4" />
    <stop offset="100%" style="stop-opacity:0" />
    </linearGradient>

    <linearGradient id="grad2" x1="70%" y1="-20%" x2="120%" y2="30%" >
    <stop offset="0%" style="stop-opacity:0.4" />
    <stop offset="80%" style="stop-opacity:0" />
    </linearGradient>
    <polygon points="60,29 80,50 80,215 60, 205" fill="url(#grad1)" color="black" stroke-width="none" stroke="none"/>
    <circle cx="50" cy="40" r="14.5" fill="white" />
    <polygon points="70,208 100,238 70,288 32,254" fill="url(#grad2)" color="black" stroke="none"/>
    <rect width="30" height="200" x="35" y="40" fill="white" stroke="none"></rect>

    <circle cx="50" cy="230" r="30" stroke="none" stroke-width="3" fill="white" />

    <rect width="16" height="179" x="-58" y="-210" :class="IsAbove ? 'redThermoBackground':'blueThermoBackground'" stroke="none" transform="rotate(180)"></rect>
    <rect width="16" :height="termo_height" x="-58" y="-210" :class="IsAbove ? 'redThermo':'blueThermo'" stroke="none" transform="rotate(180)"></rect>
    <path d="M40 40 A 4 4 7 0 1 60 40" stroke="white" stroke-width="4" fill="none"></path>
    <polygon points="66,215 85,234 68,274 39,249" fill="url(#grad2)" color="black" stroke="none"/>
    <polygon :points="comp_shadow" fill="url(#grad1)" color="black" stroke-width="none" stroke="none"/>
    <circle cx="50" cy="230" r="22" stroke="none" stroke-width="3" :class="IsAbove ? 'redThermo':'blueThermo'"/>


    <text :x="width_display" y="237" font-family="Tahoma" font-size="16" fill="white" stroke="none">{{this.man ? this.man.toString() + "°" : this.temp.toString() + "°" }}</text>

    <rect width="23" height="2" x="35" y="202" fill="white" stroke="none"></rect>
    <rect width="15" height="2" x="70" y="202" fill="black" stroke="none"></rect>
    <text x="90" y="204" font-family="Tahoma" font-size="10" fill="black" stroke="none">-30</text>

    <rect width="7.5" height="2" x="70" y="192" fill="black" stroke="none"></rect>

    <rect width="23" height="2" x="35" y="182" fill="white" stroke="none"></rect>
    <rect width="15" height="2" x="70" y="182" fill="black" stroke="none"></rect>
    <text x="90" y="184" font-family="Tahoma" font-size="10" fill="black" stroke="none">-20</text>

    <rect width="7.5" height="2" x="70" y="172" fill="black" stroke="none"></rect>

    <rect width="23" height="2" x="35" y="162" fill="white" stroke="none"></rect>
    <rect width="15" height="2" x="70" y="162" fill="black" stroke="none"></rect>
    <text x="90" y="164" font-family="Tahoma" font-size="10" fill="black" stroke="none">-10</text>

    <rect width="7.5" height="2" x="70" y="152" fill="black" stroke="none"></rect>

    <rect width="23" height="2" x="35" y="142" fill="white" stroke="none"></rect>
    <rect width="15" height="2" x="70" y="142" fill="black" stroke="none"></rect>
    <text x="99" y="144" font-family="Tahoma" font-size="10" fill="black" stroke="none">0</text>

    <rect width="7.5" height="2" x="70" y="132" fill="black" stroke="none"></rect>

    <rect width="23" height="2" x="35" y="122" fill="white" stroke="none"></rect>
    <rect width="15" height="2" x="70" y="122" fill="black" stroke="none"></rect>
    <text x="94" y="124" font-family="Tahoma" font-size="10" fill="black" stroke="none">10</text>

    <rect width="7.5" height="2" x="70" y="112" fill="black" stroke="none"></rect>

    <rect width="23" height="2" x="35" y="102" fill="white" stroke="none"></rect>
    <rect width="15" height="2" x="70" y="102" fill="black" stroke="none"></rect>
    <text x="94" y="104" font-family="Tahoma" font-size="10" fill="black" stroke="none">20</text>

    <rect width="7.5" height="2" x="70" y="92" fill="black" stroke="none"></rect>

    <rect width="23" height="2" x="35" y="82" fill="white" stroke="none"></rect>
    <rect width="15" height="2" x="70" y="82" fill="black" stroke="none"></rect>
    <text x="94" y="84" font-family="Tahoma" font-size="10" fill="black" stroke="none">30</text>

    <rect width="7.5" height="2" x="70" y="72" fill="black" stroke="none"></rect>

    <rect width="23" height="2" x="35" y="62" fill="white" stroke="none"></rect>
    <rect width="15" height="2" x="70" y="62" fill="black" stroke="none"></rect>
    <text x="94" y="64" font-family="Tahoma" font-size="10" fill="black" stroke="none">40</text>

    <rect width="7.5" height="2" x="70" y="52" fill="black" stroke="none"></rect>

    <rect width="23" height="2" x="35" y="42" fill="white" stroke="none"></rect>
    <rect width="15" height="2" x="70" y="42" fill="black" stroke="none"></rect>
    <text x="94" y="44" font-family="Tahoma" font-size="10" fill="black" stroke="none">50</text>

    <text x="64" y="34" font-family="Tahoma" font-size="20" fill="black" stroke="none">°C</text>
    </svg>
  </div>
</template>

<script>
export default {
  name: 'Thermometer',
  props: {
    temp: Number,
    fl: Number,
    airTemp: String,
    rainm: Number,
    man: null,
  },
  data: function () {
      return {
            display: '',

      }
   },
  computed: {

          SnowRaining() {
            if (this.rainm > 0) {
                return true;
            } else {
                return false;
            }
          },

          IsAbove() {
            if (this.man) {
                if (2 * this.man + 67 < 67.0) {
                    return false;
                } else {
                    return true;
                }
            } else {
                if (2 * this.temp + 67 < 67.0) {
                    return false
                } else {
                    return true
                }
            }
          },

          DisplayTemp() {
                if (this.man) {
                    return this.man.toString() + "°";
                } else {
                    return this.temp.toString() + "°";
                }

          },

          termo_height() {
              if (this.temp) {
                  if (this.man) {
                      return 2 * this.man + 67;
                  }
                  return 2 * this.temp + 67;
              }
              return 0;
          },

          width_display() {
          let len = 0;
          if (this.man) {
            len = (this.man.toString() + "°").length;
          } else {
            len = (this.temp.toString() + "°").length;
          }
                      switch (len) {
                          case 2:
                              return 42;
                          case 3:
                              return 38;
                          case 4:
                              return 36;
                          case 5:
                              return 32;
                          default:
                              return 28;
                      }
          },
          comp_shadow() {
              let a = null;
              let b = null;
                  if (this.man) {
                      if (this.man < 53.0) {
                          a = (204 - (2 * this.man + 61));
                          b = (224 - (2 * this.man + 61));
                          return '58,' + a + ' 78,' + b + ' 78,227 58, 210';
                      } else {
                          a = (204 - (2 * 53 + 60));
                          b = (224 - (2 * 53 + 60));
                          return '58,' + a + ' 78,' + b + ' 78,227 58, 210';
                      }
                  } else {
                      if (this.temp < 53.0) {
                          a = (204 - (2 * this.temp + 61));
                          b = (224 - (2 * this.temp + 61));
                          return '58,' + a + ' 78,' + b + ' 78,227 58, 210';
                      } else {
                          a = (204 - (2 * 53 + 60));
                          b = (224 - (2 * 53 + 60));
                          return '58,' + a + ' 78,' + b + ' 78,227 58, 210';
                      }
                  }
          }
      },
   methods: {
        disply: function() {
            if (this.man) {
                return this.manual.toString() + "°";
            } else {
                return this.temp.toString() + "°";
            }
        },
   }

}
</script>


<style scoped>
.redThermoBackground {
    fill: #FFBDBD;
}

.blueThermoBackground {
    fill: #BDBDFF;
}

.redThermo {
    fill: #FF0000;
}

.blueThermo {
    fill: #0000FF;
}


</style>
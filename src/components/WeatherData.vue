<template>
  <div
      class="weather"
      :class="{'weather--upload': upload}"
  >
    <div class="weather__main">
      <img class="weather__icon weather__icon--big" v-if="icon" :src="`https://openweathermap.org/img/wn/${icon}.png`"
           alt="weather status"/>
      <span class="weather__temp">{{ temp }}°C</span>
    </div>
    <span class="weather__info bold">Feels like {{ feelsLike }}°C. {{ clouds }}.</span>
    <div class="weather__info">
      <span class="weather__sensor">
        <svg class="weather__icon weather__icon--small" :style="`transform: rotate(${windDeg}deg)`"
             viewBox="0 0 1000 1000" xml:space="preserve"><g fill="#48484a"><path d="M510.5,749.6c-14.9-9.9-38.1-9.9-53.1,1.7l-262,207.3c-14.9,11.6-21.6,6.6-14.9-11.6L474,48.1c5-16.6,14.9-18.2,21.6,0l325,898.7c6.6,16.6-1.7,23.2-14.9,11.6L510.5,749.6z"></path><path
            d="M817.2,990c-8.3,0-16.6-3.3-26.5-9.9L497.2,769.5c-5-3.3-18.2-3.3-23.2,0L210.3,976.7c-19.9,16.6-41.5,14.9-51.4,0c-6.6-9.9-8.3-21.6-3.3-38.1L449.1,39.8C459,13.3,477.3,10,483.9,10c6.6,0,24.9,3.3,34.8,29.8l325,898.7c5,14.9,5,28.2-1.7,38.1C837.1,985,827.2,990,817.2,990z M485.6,716.4c14.9,0,28.2,5,39.8,11.6l255.4,182.4L485.6,92.9l-267,814.2l223.9-177.4C454.1,721.4,469,716.4,485.6,716.4z"></path></g></svg>
        <span>{{ windSpeed }}m/s</span>
      </span>
      <span class="weather__sensor">
        <svg class="weather__icon weather__icon--small" width="16pt" height="16pt"
             viewBox="0 0 96 96">
          <g transform="translate(0,96) scale(0.100000,-0.100000)" fill="#48484a" stroke="none">
            <path d="M351 854 c-98 -35 -179 -108 -227 -202 -27 -53 -29 -65 -29 -172 0
      -107 2 -119 29 -172 38 -75 104 -141 180 -181 58 -31 66 -32 176 -32 110 0
      118 1 175 32 77 40 138 101 178 178 31 57 32 65 32 175 0 110 -1 118 -32 176
      -40 76 -106 142 -181 179 -49 25 -71 29 -157 32 -73 2 -112 -1 -144 -13z m259
      -80 c73 -34 126 -86 161 -159 24 -50 29 -73 29 -135 0 -62 -5 -85 -29 -135
      -57 -119 -161 -185 -291 -185 -130 0 -234 66 -291 185 -24 50 -29 73 -29 135
      0 130 66 234 185 291 82 40 184 41 265 3z"></path>
            <path d="M545 600 c-35 -35 -68 -60 -80 -60 -27 0 -45 -18 -45 -45 0 -33 -50
      -75 -89 -75 -18 0 -41 -5 -53 -11 -20 -11 -20 -11 3 -35 12 -13 33 -24 46 -24
      17 0 23 -6 23 -23 0 -13 10 -33 23 -45 30 -28 47 -13 47 43 0 32 6 47 28 68
      15 15 37 27 48 27 26 0 44 18 44 44 0 12 26 47 60 81 l60 61 -28 27 -28 27
      -59 -60z"></path>
          </g>
        </svg>
        <span>{{ pressure }}hpa</span>
      </span>
    </div>
    <span class="weather__info">Humidity: {{ humidity }}%</span>
    <span class="weather__info">Dew point: {{ dewPoint }}°C</span>
    <span class="weather__info">Visibility: {{ visibility }}km</span>
  </div>
</template>

<script lang="ts">
import {defineComponent} from "vue"

export default defineComponent({
  name: "WeatherData",
  data() {
    return {
      icon: '',
      temp: 0,
      feelsLike: 0,
      clouds: '',
      windSpeed: '',
      windDeg: '',
      pressure: '',
      humidity: '',
      dewPoint: 0,
      visibility: 0,
      upload: false
    }
  },
  props: {
    lat: {
      type: Number,
      default: 55.7522
    },
    lon: {
      type: Number,
      default: 37.615
    }
  },
  mounted() {
    this.showWeather();
  },
  watch: {
    lat: 'showWeather'
  },
  methods: {
    showWeather() {
      this.show('e050de9385462b546f2d3a2143ebb69f', this.lat, this.lon, 'ru')
    },
    async show(token: string, lat: number, lon: number, lang: string) {
      this.upload = true
      await fetch(`https://api.openweathermap.org/data/2.5/onecall?lat=${lat}&lon=${lon}&APPID=${token}&units=metric&lang=${lang}`)
          .then(res => res.json())
          .then(body => {
            const current = body.current
            this.temp = Math.round(current.temp)
            this.feelsLike = Math.round(current.feels_like)
            this.windSpeed = current.wind_speed
            this.windDeg = current.wind_deg
            this.pressure = current.pressure
            this.humidity = current.humidity
            this.dewPoint = Math.round(current.dew_point)
            this.visibility = current.visibility / 1000
            this.clouds = current.weather[0].main
            this.icon = current.weather[0].icon
          })
          .catch(err => console.log(err))
      this.upload = false
    }
  }
})
</script>

<style lang="scss" scoped>
.weather {
  &__main {
    display: flex;
    align-items: center;
    justify-content: flex-start;
    margin: 0 0 10px;
  }

  &__icon {
    display: block;

    &--small {
      display: inline-block;
      width: 1em;
      margin-right: 5px;
    }
  }

  &__temp {
    font-size: 2em;
    font-weight: 500;
  }

  &__info {
    display: flex;
    align-items: center;
    text-align: left;
    margin: 0 0 8px;

    &:last-child {
      margin: 0;
    }
  }

  &__sensor {
    display: flex;
    align-items: center;
    margin-right: 10px;
  }

  &--upload {
    opacity: 0.3;

    &:after {
      content: '';
      display: block;
      position: absolute;
      top: 50%;
      left: 50%;
      width: 50px;
      height: 50px;
      margin-top: -25px;
      margin-left: -25px;
      border: 5px solid grey;
      border-left-color: rgba(255, 255, 255, 0.2);
      border-right-color: rgba(255, 255, 255, 0.2);
      border-radius: 50%;
      animation: animation-rotate 1s linear infinite;
      z-index: 1;
    }
  }
}

@keyframes animation-rotate {
  0% {
    transform: rotateZ(0);
  }
  100% {
    transform: rotateZ(360deg);
  }
}
</style>
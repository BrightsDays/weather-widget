<template>
  <div class="app">
    <city-choose @changeLat="onChangeLat" @changeLon="onChangeLon" />
    <weather-data :lat="lat" :lon="lon" />
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import WeatherData from "@/components/WeatherData.vue";
import CityChoose from "@/components/CityChoose.vue";

export default defineComponent({
  name: 'App',
  components: {
    CityChoose,
    WeatherData
  },
  data() {
    return {
      lat: 0,
      lon: 0
    }
  },
  methods: {
    onChangeLat(value: {type: number}) {
      this.lat = value;
    },
    onChangeLon(value: {type: number}) {
      this.lon = value;
    }
  }
});
</script>

<style lang="scss">
.app {
  width: 290px;
  margin: 20px auto;
  padding: 15px;
  max-width: 100%;
  border: 1px solid grey;
  border-radius: 8px;
}

* {
  margin: 0;
  padding: 0;
  font-family: Ubuntu,serif;
  font-size: 16px;
}

.bold {
  font-weight: 500;
}
</style>

<template>
  <div class="app">
    <city-choose />
    <weather-data />
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
  }
});
</script>

<style lang="scss">
.app {
  width: 290px;
  margin: 0 auto;
  padding: 15px;
  max-width: 100%;
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

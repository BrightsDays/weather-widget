<template>
  <div class="city-choose">
    <div>{{ city }}, {{ country }}</div>
    <div v-if="menuIsOpen"
         class="city-choose__list">
      <button @click="getCityCoords('london', 'e050de9385462b546f2d3a2143ebb69f', 'en')">London</button>
      <button @click="getCityCoords('moscow', 'e050de9385462b546f2d3a2143ebb69f', 'ru')">Moscow</button>
      <button @click="getCityCoords('berlin', 'e050de9385462b546f2d3a2143ebb69f', 'de')">Berlin</button>
    </div>
    <button class="city-choose__burger"
            :class="{'city-choose__burger--close': menuIsOpen}"
            @click="toggleMenu()">
      <div/>
    </button>
  </div>
</template>

<script lang="ts">
export default {
  name: 'CityChoose',
  data() {
    return {
      city: '',
      country: '',
      menuIsOpen: false
    }
  },
  emits: ['changeLat', 'lon'],
  mounted() {
    this.getCityCoords('moscow', 'e050de9385462b546f2d3a2143ebb69f', 'ru')
  },
  methods: {
    getCityCoords(selector, token, lang) {
      fetch(`http://api.openweathermap.org/data/2.5/weather?q=${selector}&appid=${token}&lang=${lang}`)
          .then(res => res.json())
          .then(body => {
            this.city = body.name;
            this.country = body.sys.country;
            this.$emit("changeLat", body.coord.lat);
            this.$emit("changeLon", body.coord.lon);
          })
    },
    toggleMenu() {
      this.menuIsOpen = !this.menuIsOpen;
    }
  }
}
</script>

<style lang="scss">
.city-choose {
  position: relative;

  &__burger {
    position: absolute;
    top: 0;
    right: 0;
    display: flex;
    justify-content: center;
    align-items: center;
    width: 16px;
    height: 16px;
    cursor: pointer;
    border: none;
    background: none;
    z-index: 1;

    div {
      position: absolute;
      width: 100%;
      border-top: 1.6px solid #000000;
      transition: transform 0.3s;

      &:before {
        position: absolute;
        top: -7px;
        left: 0;
        width: 16px;
        content: '';
        border-top: 1.6px solid #000000;
        transition: transform 0.3s;
      }

      &:after {
        position: absolute;
        bottom: -5px;
        left: 0;
        width: 16px;
        content: '';
        border-top: 1.6px solid #000000;
        transition: transform 0.3s;
      }
    }

    &--close {
      div {
        border: none;
        transform: translateY(-4px) translateX(-1.5px);

        &:before {
          width: 20px;
          transform: translateY(10px) rotate(45deg);
        }

        &:after {
          width: 20px;
          transform: rotate(-45deg);
        }
      }
    }
  }
}
</style>
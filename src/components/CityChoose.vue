<template>
  <div
      class="city-choose"
      :class="{'city-choose--menu-shown': menuIsOpen}"
  >
    <div v-if="city && !menuIsOpen">
      {{ city }}, {{ country }}
    </div>
    <div
        v-if="menuIsOpen"
        class="city-choose__list"
    >
      <button
          class="city-choose__item"
          v-for="item in citiesList"
          :key="item"
          @click="getCityCoords(item, 'e050de9385462b546f2d3a2143ebb69f', 'en')"
          >{{ item }}</button>
    </div>
    <button
        class="city-choose__burger"
        :class="{'city-choose__burger--close': menuIsOpen}"
        @click="toggleMenu()"
    >
      <span/>
    </button>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'

export default defineComponent({
  name: 'CityChoose',
  data() {
    return {
      city: '',
      country: '',
      citiesList: [
        'Saint Petersburg',
        'Belgrade',
        'Moscow',
        'Helsinki',
        'Kazan'
      ],
      menuIsOpen: false
    }
  },
  emits: {
    changeLat(crd: number): number {
      return crd
    },
    changeLon(crd: number): number {
      return crd
    }
  },
  mounted() {
    this.getCityCoords('moscow', 'e050de9385462b546f2d3a2143ebb69f', 'en')
  },
  methods: {
    getCityCoords(selector: string, token: string, lang: string): void {
      fetch(`https://api.openweathermap.org/data/2.5/weather?q=${selector}&appid=${token}&lang=${lang}`)
        .then(res => res.json())
        .then(body => {
          this.city = body.name
          this.country = body.sys?.country
          this.$emit("changeLat", body.coord.lat)
          this.$emit("changeLon", body.coord.lon)
        })
        .then((): void => this.toggleMenu())
    },
    toggleMenu (): void {
      this.menuIsOpen = !this.menuIsOpen
    }
  }
})
</script>

<style lang="scss">
.city-choose {
  position: absolute;
  top: 15px;
  width: calc(100% - 30px);
  z-index: 1;

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

    span {
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
        bottom: -6px;
        left: 0;
        width: 16px;
        content: '';
        border-top: 1.6px solid #000000;
        transition: transform 0.3s;
      }
    }

    &--close {
      span {
        border: none;
        transform: translateY(-4px) translateX(-3.5px);

        &:before {
          width: 20px;
          top: -5.5px;
          transform: translateY(10px) rotate(45deg);
        }

        &:after {
          width: 20px;
          transform: rotate(-45deg);
        }
      }
    }
  }

  &__list {
    display: flex;
    flex-direction: column;
    height: 100%;
    background: #ffffff;
  }

  &__item {
    padding: 10px 0;
    background: none;
    border: none;
    text-align: left;
    cursor: pointer;
  }

  &--menu-shown {
    height: calc(100% - 30px);
  }
}
</style>
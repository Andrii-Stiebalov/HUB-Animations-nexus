<template>
<div class="Animations-nav">
  <div class="Animations-nav__list">
    <div 
      class="Animations-nav__item"
      v-for="{categoryName, iconId, name} in categories" 
      :key="categoryName"
      @click="onHendleChooseCategory(categoryName)"
      :class="{'Animations-nav__item--isActive': categoryName === currentCategoryId}"
    >
      <div class="Animations-nav__icon" 
        :style="{
          '-webkit-mask': `url(${addUrl(iconId)}) no-repeat center`,
          'mask': `url(${addUrl(iconId)}) no-repeat center`,
        }"></div>
      <p>{{name}}</p>
    </div>
  </div>
</div>
</template>

<script>
import { categories } from '../assets/categories.json'

export default {
  name: 'Animations-nav',
  props: {
    currentCategoryId: String,
    onHendleChooseCategory: Function
  },

  data() {
    return {
      categories
    }
  },

  methods: {
    addUrl(name) {
      return require(`../assets/animation_icons/${name}.svg`);
    },
  }
}
</script>

<style lang="scss" scoped>
  $width: calc(100vw / 16);
.Animations-nav {
  p {
    margin: 0;
  }

  &__list {
    display: flex;
    flex-direction: column;
    justify-content: space-between;

    height: calc(100vh - 90px);
    margin: 45px 0;
  }

  &__icon {
    width: calc($width * 0.5);
    height: calc($width * 0.5);
    max-width: calc(80px * 0.5);
    max-height: calc(80px * 0.5);
    background-color: #E5FFFF;
    mask-size: contain;
  
    opacity: 0.25;
    transition: opacity 0.3s;
  }

  &__item {
    color: rgba(229, 255, 255, 0.4);
    font-size: 12px;
    line-height: 15px;
    padding: 15px 0;
    width: calc(9.25vw);
    cursor: pointer;

    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;

    &--isActive {
      background: linear-gradient(
        270.04deg,
        rgba(110, 122, 122, 0) 0%,
        #6a787a40 50%,
        rgba(110, 122, 122, 0) 100%);

      .Animations-nav__icon {
        transition: opacity 0.3s;
        background-position: 0%;
        opacity: 1;
      }
    }
  }
}
</style>
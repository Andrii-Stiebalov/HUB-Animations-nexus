<template>
<div 
    class="Animation" 
    tabindex="0" 
  >
    <div class="Animation__box" >
      <div class="Animation__index">{{index}}</div>
      <div
        v-if="animation" 
        class="Animation__icon"
        :style="{
          '-webkit-mask': `url(${addUrl(animation.iconId)}) no-repeat center`,
          'mask': `url(${addUrl(animation.iconId)}) no-repeat center`,
        }"
      ></div>
    </div>
  <h4 class="Animation__type">{{animation.type}}</h4>
  <h5 class="Animation__name">{{animation.name}}</h5>
</div>
</template>

<script>
export default {
  props: {
    animation: Object,
    isSelected: Boolean,
    index: Number,
    startDrag: Function
  },

  methods: {
    addUrl(iconId) {
      return require(`../assets/animation_icons/${iconId}.svg`);
    }
  }
}
</script>

<style lang="scss">
$width: calc(100vw / 16);

.Animation {
  width: fit-content;
  cursor: pointer;
  margin-right: 10px;

  &__box {
    position: relative;
    box-sizing: border-box;
    display: flex;
    justify-content: center;
    align-items: center;
    width: $width;
    max-height: 76px;
    max-width: 76px;
    height: $width;
    border: 1px solid transparent;
    background-color: rgba(19, 26, 27, 0.7);

    &--empty {
      border-image: linear-gradient(
          45deg,
        rgba(43, 217, 217, 0.25) 0%,
        rgba(43, 217, 217, 0) 49.21%,
        rgba(43, 217, 217, 0.25) 100%,
        ) 1;
    }
  }

  &__delete-button{
    position: absolute;
    top:0;
    right: 0;
    background-color: transparent;
    border: none;
  }

  &__index {
    font-style: italic;
    font-weight: 600;
    font-size: 18px;
    color: rgba(229, 255, 255, 0.15);
    position: absolute;
    bottom:0;
    padding-right: 5px;
    right: 0;
  }

  &__type {
    font-size: 12px;
    font-weight: 500;
    text-align: center;
    color: #E5FFFF;
    margin: 5px 0 0;
  }

  &__name {
    font-size: 10px;
    font-weight: 600;
    text-align: center;
    color: #E5FFFF66;
    margin: 0;
    padding: 0;
  }

  &__icon {
    width: calc($width * 0.5);
    height: calc($width * 0.5);
    max-width: calc(80px * 0.5);
    max-height: calc(80px * 0.5);
    background-color: #E5FFFF;
    mask-size: contain;
  
    opacity: 0.25;
  }

  &:focus {
    .Animation {
      &__box {
        border-image: linear-gradient(
          45deg,
        rgba(43, 217, 217, 0.25) 0%,
        rgba(43, 217, 217, 0) 49.21%,
        rgba(43, 217, 217, 0.25) 100%,
        ) 1;

      &::after {
        content: '';
        display: block;
        position: absolute;
        top: 0;
        bottom: 0;
        left:0;
        right: 0;

        background: radial-gradient(50% 50% at 50% 50%, rgba(43, 217, 217, 0.25) 0%, rgba(43, 217, 217, 0) 100%);
        opacity: 0.25;
      }
    }

      &__icon {
        background: #2BD9D9;
        opacity: 1;
      }
    }
  }
}
</style>
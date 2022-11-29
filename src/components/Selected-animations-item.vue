<template>
    <draggable 
      @add="onDrop($event, index)"
      group="animations"
      :item-key="key => key"
      ghost-class="ghost-class"
      class="Animation__container"
    >
    <div 
        class="Animation" 
        tabindex="0" 
      >
        <div class="Animation__box Animation__box--empty">
          <button class="Animation__delete-button" @click="removeAnimation(index)" >
            <svg width="8" height="8" viewBox="0 0 8 8" fill="none" xmlns="http://www.w3.org/2000/svg">
              <path d="M5.32003 4.00023L7.79489 1.52538C8.06837 1.2519 8.06837 0.80854 7.79489 0.535527L7.46494 0.205577C7.19146 -0.0679034 6.7481 -0.0679034 6.47509 0.205577L4.00023 2.68043L1.52538 0.20511C1.2519 -0.0683701 0.80854 -0.0683701 0.535527 0.20511L0.20511 0.53506C-0.0683701 0.80854 -0.0683701 1.2519 0.20511 1.52491L2.68043 4.00023L0.205577 6.47509C-0.0679034 6.74857 -0.0679034 7.19193 0.205577 7.46494L0.535527 7.79489C0.809007 8.06837 1.25236 8.06837 1.52538 7.79489L4.00023 5.32003L6.47509 7.79489C6.74857 8.06837 7.19193 8.06837 7.46494 7.79489L7.79489 7.46494C8.06837 7.19146 8.06837 6.7481 7.79489 6.47509L5.32003 4.00023Z" fill="#E5FFFF" fill-opacity="0.15"/>
            </svg>
          </button>
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
      <h4 class="Animation__type">{{animation ? animation.type : "Анимация"}}</h4>
      <h5 class="Animation__name">{{animation ? animation.name : "Отсутсвует"}}</h5>
    </div>
  </draggable>
  </template>
  
  <script>
  import { VueDraggableNext } from 'vue-draggable-next'



  export default {
    data(){
      return {
        
      }
    },
    props: {
      animation: Object,
      isSelected: Boolean,
      index: String,
      onDrop: Function,
      removeAnimation: Function
    },
  
    methods: {
      addUrl(iconId) {
        return require(`../assets/animation_icons/${iconId}.svg`);
      }
    },
    components: {
    draggable: VueDraggableNext,
  },
}
  </script>
  
  <style lang="scss">
  $width: calc(100vw / 16);

  .ghost-class{
    display: none;
  }
  
  .Animation {
    width: fit-content;
    cursor: pointer;

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
      z-index: 3;
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
      max-width: calc(78px * 0.5);
      max-height: calc(78px * 0.5);
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
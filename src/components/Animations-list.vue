<!-- eslint-disable vue/no-use-v-if-with-v-for -->
<template>
  <div 
    class="Animations-list__container" 
    :class="{'Animations-list__container--forSelection': isSelectedSection}"
  >
    <draggable
      v-if="!isSelectedSection"
      :list="animations"
      :group="{ name: 'animations', pull: 'clone', put: false }"
      class="Animations-list"
      :class="{'Animations-list--forSelection': isSelectedSection}"
      ghost-class="ghost"
    >
      <Animation_item 
        v-for="animation in animations" 
        :key="animation" 
        :animation="animation"
        :isSelected="isSelectedSection"
      />
      <div class="empty"></div>
      </draggable>
      <div 
        class="Animations-list" 
        v-if="isSelectedSection"
      > 
        <Selected_animations_item
          v-for="(value, key) in animations" 
          :key="animation ? animation.id : key"
          :animation="value"
          :index="key"
          :isSelected="isSelectedSection"
          :onDrop="onDrop"
          :removeAnimation="removeAnimation"
        />
      </div>
  </div>
</template>

<script>
import Animation_item from './Animation-item.vue';
import Selected_animations_item from './Selected-animations-item.vue';
import { VueDraggableNext } from 'vue-draggable-next'

export default {
  props: {
    animations: Object,
    isSelectedSection: Boolean,
    onDrop: Function,
    removeAnimation: Function
  },

  components: {
    Animation_item,
    Selected_animations_item,
    draggable: VueDraggableNext,
  },

  methods: {
    handleMove(e) {
      const { index, futureIndex } = e.draggedContext;
      this.movingIndex = index;
      console.log(index, futureIndex)
      this.futureIndex = futureIndex;
      return false;
    }
  },
}
</script>

<style lang="scss" scoped>
$width: calc(100vw / 16);
.empty {
  width: $width;
  max-width: 80px;
}
.Animations-list {
    display: flex;
    gap: 10px;

    justify-content: space-between;
    align-items: center;
    flex-wrap: wrap;
    flex-basis: $width;
    max-height: calc(100vh / 2.2);
    
    overflow: scroll;
    overflow-x: hidden;
    
    &::-webkit-scrollbar {
      width: 2px;
      margin-left: -20px;
    }

    &::-webkit-scrollbar-track {
      background:rgba(229, 255, 255, 0.07);
    }

    &::-webkit-scrollbar-thumb {
      background-color: #2BD9D9;
    }

    &--forSelection {
      max-height: 100px;
    }

    &__container {
      width: calc(100% + 10px);
      min-height: calc(100vh / 2.2);
      align-self: flex-start;

    &--forSelection{ 
      min-height: 250px;
    }
  }
}

</style>
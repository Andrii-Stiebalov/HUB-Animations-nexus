<template>
  <div class="HUD-Animations">
    <div class="HUD-Animations__main">
      <form 
        class="HUD-Animations__search-bar" 
        @submit="event => event.preventDefault()" 
      >
        <input 
          type="text" 
          name="serchForName" 
          placeholder="Поиск по названию"
          class="HUD-Animations__search-bar_input"
          @input="onChangeInput($event)"
          :value="qwery"
        >
        <svg width="23" height="23" viewBox="0 0 23 23" fill="none" xmlns="http://www.w3.org/2000/svg" class="HUD-Animations__search-bar_search-line">
          <g clip-path="url(#clip0_1_595)">
            <path d="M10.4331 3.22641C11.817 3.22515 13.1702 3.63437 14.3215 4.40231C15.4728 5.17026 16.3704 6.26241 16.9009 7.54062C17.4314 8.81883 17.5709 10.2257 17.3016 11.5831C17.0324 12.9406 16.3666 14.1878 15.3885 15.1668C14.4104 16.1458 13.1639 16.8127 11.8066 17.0832C10.4494 17.3536 9.04243 17.2155 7.76374 16.6862C6.48505 16.1568 5.39207 15.2602 4.62307 14.1096C3.85408 12.959 3.44362 11.6062 3.44362 10.2222C3.45201 8.37052 4.19088 6.59694 5.49966 5.28696C6.80844 3.97699 8.58135 3.23649 10.4331 3.22641ZM10.4331 1.91669C8.79038 1.91669 7.18458 2.4038 5.81874 3.31643C4.4529 4.22905 3.38836 5.5262 2.75973 7.04384C2.1311 8.56149 1.96662 10.2315 2.2871 11.8426C2.60757 13.4537 3.39859 14.9336 4.56015 16.0952C5.7217 17.2567 7.20161 18.0477 8.81273 18.3682C10.4238 18.6887 12.0938 18.5242 13.6115 17.8956C15.1291 17.2669 16.4263 16.2024 17.3389 14.8366C18.2515 13.4707 18.7386 11.8649 18.7386 10.2222C18.7386 8.01947 17.8636 5.90692 16.306 4.34933C14.7484 2.79173 12.6358 1.91669 10.4331 1.91669Z"/>
            <path d="M22.3611 21.2686L17.6525 16.5281L16.7453 17.4289L21.4539 22.1695C21.5131 22.229 21.5834 22.2764 21.6608 22.3088C21.7383 22.3412 21.8213 22.358 21.9053 22.3583C21.9892 22.3586 22.0724 22.3423 22.1501 22.3105C22.2277 22.2786 22.2984 22.2318 22.3579 22.1727C22.4175 22.1135 22.4648 22.0432 22.4972 21.9658C22.5296 21.8883 22.5465 21.8053 22.5468 21.7213C22.5471 21.6374 22.5308 21.5542 22.499 21.4765C22.4671 21.3988 22.4203 21.3282 22.3611 21.2686Z" />
            </g>
            <defs>
            <clipPath id="clip0_1_595">
            <rect width="23" height="23" fill="white"/>
            </clipPath>
          </defs>
        </svg>

      </form>
      <h3 class="HUD-Animations__title">Анимации</h3>

      <Animations_list 
        class="HUD-Animations__list"
        :animations="animations"
        :startDrag="startDrag"
      />
      <h3 class="HUD-Animations__title">Быстрый доступ</h3>
      <h4 class="HUD-Animations__subtitle">
        Для добавления анимации в быстрый доступ - зажмите ЛКМ и перетащите анимацию в нужную ячейку
      </h4>

      <Animations_list 
        class="HUD-Animations__list"
        :animations="selectedAnimations"
        :isSelectedSection="true"
        :onDrop="onDrop"
        :removeAnimation="removeAnimation"
        :style="{'margin-bottom': 0}"
      />
      <div class="HUD-Animations__footer">
        <h3 class="HUD-Animations__title HUD-Animations__title--footer"
        >ESC</h3>
        <h4 class="HUD-Animations__subtitle HUD-Animations__subtitle--footer"
        > - Закрыть окно</h4>
      </div>
    </div>
    <Animations_nav 
      :currentCategoryId="currentCategoryId" 
      :onHendleChooseCategory="onHendleChooseCategory"
    />
  </div>
</template>

<script>
import Animations_list from "./Animations-list.vue";
import Animations_nav from "./Animations-nav.vue";
import { animations } from '../assets/animations.json';

export default {
  data() {
    return {
      qwery: '',
      currentCategoryId: 'dances',
      selectedAnimations: {
        1: null,
        2: null,
        3: null,
        4: null,
        5: null,
        6: null
      },
      animations
    }
  },

  methods: {
    onHendleChooseCategory(id) {
      this.currentCategoryId = this.currentCategoryId !== id ? id : null;
    },
    onChangeInput(evt) {
      evt.preventDefault ();
      this.qwery = evt.target.value;
    },
    filterAnimations() {
      if (this.currentCategoryId) {
        this.animations = animations
        .filter(animation => animation.category === this.currentCategoryId);
      } else {
        this.animations = animations;
      }

      this.animations = this.animations
      .filter(animation => animation.name.toLowerCase()
      .includes(this.qwery.toLowerCase()))
    },
    onDrop(evt, index) {
      const id = evt.item._underlying_vm_ ? evt.item._underlying_vm_.id : evt.item.props.index;
      const item = this.animations.find((item) => item.id == id);
      this.selectedAnimations[index] = item;
      this.$toast.show('dragEnd')
    },
    removeAnimation(index) {
      this.selectedAnimations[index] = null;
    }
  },

  mounted() {
    this.filterAnimations()
  },

  watch: {
    currentCategoryId(){
      this.filterAnimations()
    },
    qwery(){
      this.filterAnimations()
    } 
  },

  components: {
    Animations_list,
    Animations_nav,
  }
}
</script>

<style scoped lang="scss">
.HUD-Animations{
  position: absolute;
  top:0;
  bottom: 0;
  right: 0;
  left: 50%;

  background: linear-gradient(270deg, #131A1BF2 0%, #131A1BF2 50%, rgba(19, 26, 27, 0) 100%);
  display: flex;
  flex-direction: row-reverse;

  &__main {
    padding: 20px;
    box-sizing: border-box;
    width: 40.75%;
    display: flex;
    align-items: center;
    flex-direction: column;
    border-left: 2px solid;
    border-image: linear-gradient(
      to bottom,
      rgba(0, 0, 0, 0),
      rgba(229, 255, 255, 0.1),
      rgba(0, 0, 0, 0)
    ) 1 100%;
  }

  &__title {
    margin: 11px 0 0;
    font-size: 14px;
    font-weight: 500;
    line-height: 17px;
    color: rgba(229, 255, 255, 1);

    &--footer{
      margin: 0;
    }
  }

  &__subtitle {
    margin: 5px 0 0;
    font-size: 12px;
    font-weight: 500;
    color: #E5FFFF33;
    text-align: center;
    color: rgba(229, 255, 255, 0.2);

    &--footer{
      margin: 0;
      padding-left: 5px;
    }
  }

  &__list {
    margin: 15px 0;
    &:last-child {
      margin-bottom: 0;
    } 
  }

  &__footer {
    display: flex;
    justify-content: center;
    align-items: center;
  }

  &__search-bar {
    margin-top: 18px;
    width: 100%;
    position: relative;

    &_search-line {
      fill: rgba(229, 255, 255, 0.29);
      position: absolute;
      right: 15px;
      top: 10px;
    }
    
    &_input {
      padding: 13px 9px;
      box-sizing: border-box;
      font-family: inherit;

      position: relative;

      display: block;
      background: rgba(255, 255, 255, 0.03);
      border: 1px solid rgba(255, 255, 255, 0.03);

      color: #E5FFFF;
      font-size: 13px;
      width: 100%;


      &::placeholder {
        color: rgba(229, 255, 255, 0.29);
      }

      &:focus {
        outline: 0;
        .HUD-Animations__search-bar_search-line {
          background-color: aqua;
        }
      }
    }
  }
}
</style>

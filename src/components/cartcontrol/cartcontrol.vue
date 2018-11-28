<template>
  <div class="cartControl">
    <transition name="move">
      <div class="move-wrapper" v-if="show">
        <div class="cart-decrease" v-show="food.count>0" @click="decreaseCart">
          <span class="inner icon-remove_circle_outline"></span>
        </div>
        <div class="cart-count" v-show="food.count>0">{{food.count}}</div>
      </div>
    </transition>
    <div class="cart-add" @click="addCart">
      <span class="inner icon-add_circle"></span>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  import Vue from 'vue';
  export default {
    data() {
      return {
        show: false
      };
    },
    props: {
      food: {
        type: Object
      }
    },
    methods: {
      addCart(event) {
        if (!event._constructed) {
          return;
        }
        this.show = true;
        if (!this.food.count) {
          Vue.set(this.food, 'count', 1);
        } else {
          this.food.count++;
        }
        this.$emit('cartAdd', event.target);
      },
      decreaseCart(event) {
        if (!event._constructed) {
          return;
        }
        if (this.food.count) {
          this.food.count--;
        }
        if (this.food.count < 1) {
          this.show = false;
        }
      }
    }
  };
</script>

<style scoped lang="stylus">
  .cartControl
    font-size: 0
    .move-enter-active, .move-leave-active
      transition: all .5s
      .inner
        transition: all .5s
        transform: rotate(0)
    .move-enter, .move-leave-active
      opacity: 0
      transform: translate3d(24px, 0, 0)
      .inner
        transform: rotate(180deg)
    .move-wrapper
      display: inline-block
      .cart-decrease
        display: inline-block
        padding: 6px
        .inner
          display: inline-block
          line-height: 24px
          font-size: 24px
          color: rgb(0, 160, 220)
      .cart-count
        display: inline-block
        vertical-align: top
        line-height: 24px
        padding-top: 6px
        font-size: 10px
        text-align: center
        color: rgb(147, 153, 159)
    .cart-add
      display: inline-block
      padding: 6px
      line-height: 24px
      font-size: 24px
      color: rgb(0, 160, 220)
</style>

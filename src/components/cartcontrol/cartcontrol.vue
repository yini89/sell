<template>
  <div class="cartcontrol">
    <div class="cart-decrease" v-show="food.count>0" @click="decreaseCart" transition="move">
      <span class="inner icon-remove_circle_outline"></span>
    </div>
    <div class="cart-count" v-show="food.count>0">{{food.count}}</div>
    <div class="cart-add" @click="addCart">
      <span class="inner icon-add_circle"></span>
    </div>
  </div>
</template>

<script>
  import Vue from 'vue';
  export default {
    props: ['food'],
    methods: {
      addCart(event) {
        if (!event._constructed) {
          return;
        }
        if (!this.food.count) {
          Vue.set(this.food, 'count', 1);
        } else {
          this.food.count++;
        }
      },
      decreaseCart(event) {
        if (!event._constructed) {
          return;
        }
        if (this.food.count) {
          this.food.count--;
        }
      }
    }
  };
</script>

<style scoped lang="stylus">
  .cartcontrol
    font-size: 0
    .cart-decrease
      display: inline-block
      padding: 6px
      transition: all 0.4s linear
      &.move-transition
        opacity: 1
        transform: translate3D(0, 0, 0)
      .inner
        line-height: 24px
        font-size: 12px
        color: rgb(0, 160, 220)
       &.move-enter, &.move-leave
         opacity: 0
         transform: translate3D(24px, 0, 0)
    .cart-add
      display: inline-block
      padding: 6px
      line-height: 24px
      font-size: 12px
      color: rgb(0, 160, 220)
    .cart-count
      display: inline-block
      vertical-align: top
      height: 24px
      line-height: 24px
      width: 12px
      padding: 6px 0
      font-size: 10px
      text-align: center
      color: rgb(147, 153, 159)
</style>

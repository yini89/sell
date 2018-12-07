<template>
    <div class="shopCart">
      <div class="content" @click.stop.prevent="listShow">
        <div class="content-left">
          <div class="logo-wrapper">
            <div class="logo" :class="{highLight: totalCount>0}">
              <i class="icon-shopping_cart" :class="{highLight: totalCount>0}"></i>
            </div>
            <div v-show="totalCount>0" class="num">{{totalCount}}</div>
          </div>
          <div class="price" :class="{highLight: totalCount>0}">￥{{totalPrice}}</div>
          <div class="desc">另需配送费{{deliveryPrice}}元</div>
        </div>
        <div class="content-right" @click.stop.prevent="pay">
          <div class="pay" :class="payClass">{{payDesc}}</div>
        </div>
      </div>
      <div class="ball-container">
        <transition-group
                    name="drop"
                    @before-enter="beforeEnter"
                    @enter="enter"
                    @after-enter="afterEnter">
          <div v-for="(ball, index) in balls" :key="index" v-show="ball.show" class="ball">
            <div class="inner inner-hook"></div>
          </div>
        </transition-group>
      </div>
      <transition name="fold">
        <div class="shopCart-list" v-show="isListShow">
          <div class="list-header">
            <h1 class="title">购物车</h1>
            <span class="empty" @click="empty">清空</span>
          </div>
          <div class="list-content" ref="listContent">
            <ul>
              <li class="food border-1px" v-for="(food, index) in selectFoods" :key="index">
                <span class="name">{{food.name}}</span>
                <div class="price">
                  <span>￥{{food.count * food.price}}</span>
                </div>
                <div class="cartControl-wrapper">
                  <cartControl :food="food"></cartControl>
                </div>
              </li>
            </ul>
          </div>
        </div>
      </transition>
      <transition name="fade">
        <div class="list-mask" v-show="isListShow" @click.stop.prevent="listShow"></div>
      </transition>
    </div>
</template>

<script>
  import cartControl from '../cartcontrol/cartcontrol';
  import BScroll from 'better-scroll';
  export default {
    props: {
      selectFoods: {
        type: Array,
        default() {
          return [
            {
              price: 10,
              count: 1
            }
          ];
        }
      },
      deliveryPrice: {
        type: Number,
        default: 0
      },
      minPrice: {
        type: Number,
        default: 0
      }
    },
    components: {
      cartControl
    },
    data() {
      return {
        balls: [
          {show: false},
          {show: false},
          {show: false},
          {show: false},
          {show: false}
        ],
        dropBalls: [],
        fold: true
      };
    },
    computed: {
      totalPrice() {
        let total = 0;
        this.selectFoods.forEach((food) => {
          total += food.price * food.count;
        });
        return total;
      },
      totalCount() {
        let total = 0;
        this.selectFoods.forEach((food) => {
          total += food.count;
        });
        return total;
      },
      payDesc() {
        if (this.totalPrice === 0) {
          return `￥${this.minPrice}元起送`;
        } else if (this.totalPrice < this.minPrice) {
          let diff = this.minPrice - this.totalPrice;
          return `还差￥${diff}元起送`;
        } else {
          return `去结算`;
        }
     },
      payClass() {
        if (this.totalPrice < this.minPrice) {
          return 'not-enough';
        } else {
          return 'enough';
        }
      },
      isListShow() {
        if (!this.totalCount || this.totalPrice < this.minPrice) {
          return false;
        }
        let show = !this.fold;
        return show;
      }
    },
    methods: {
      drop(el) {
        for (let i = 0; i < this.balls.length; i++) {
          let ball = this.balls[i];
          if (!ball.show) {
            ball.show = true; //触发动画
            ball.el = el;
            this.dropBalls.push(ball);
            return;
          }
        }
      },
      listShow() {
        if (!this.totalCount || this.totalPrice < this.minPrice) {
          return;
        }
        this.fold = !this.fold;
        if (!this.fold) {
          this.$nextTick(() => {
            if (!this.scroll) {
              this.scroll = new BScroll(this.$refs.listContent, {
                click: true
              });
            } else {
              this.scroll.refresh();
            }
          });
        }
      },
      empty() {
        this.selectFoods.forEach((food) => {
          food.count = 0;
        });
      },
      pay() {
        window.alert(`去支付${this.totalPrice}元`);
      },
      beforeEnter(el) {
        let count = this.balls.length;
        while (count--) {
          let ball = this.balls[count];
          if (ball.show) {
            let rect = ball.el.getBoundingClientRect(); //该元素相对于视口的位置
            let x = rect.left - 32;
            let y = -(window.innerHeight - rect.top - 22);
            el.style.display = '';
            el.style.webkitTransform = `translate3d(0, ${y}px, 0)`;
            el.style.transform = `translate3d(0, ${y}px, 0)`;
            let inner = el.querySelector('.inner-hook');
            inner.style.webkitTransform = `translate3d(${x}px, 0, 0)`;
            inner.style.transform = `translate3d(${x}px, 0, 0)`;
          }
        }
      },
      enter(el) {
        /* eslint-disable no-unused-vars*/
        let rf = el.offsetHeight;
        this.$nextTick(() => {
          el.style.webkitTransform = 'translate3d(0, 0, 0)';
          el.style.transform = 'translate3d(0, 0, 0)';
          let inner = el.querySelector('.inner-hook');
          inner.style.webkitTransform = 'translate3d(0, 0, 0)';
          inner.style.transform = 'translate3d(0, 0, 0)';
        });
      },
      afterEnter(el) {
        let ball = this.dropBalls.shift();
        if (ball) {
          ball.show = false;
          el.style.display = 'none';
        }
      }
    }
  };
</script>

<style scoped lang="stylus">
  @import "../../common/stylus/mixin.styl";
  .shopCart
    position: fixed
    left: 0
    bottom: 0
    z-index: 50
    width: 100%
    height: 48px
    .content
      position: relative
      display: flex
      width: 100%
      height: 100%
      background: #000
      z-index: 60
      .content-left
        flex: 1
        background: #000
        .logo-wrapper
          position: relative
          top: -10px
          display: inline-block
          width: 56px
          height: 56px
          margin: 0 12px
          padding: 6px
          border-radius: 50%
          box-sizing: border-box
          background: #000
          text-align: center
          .logo
            width: 44px
            height: 44px
            border-radius: 50%
            background: rgba(255, 255, 255, 0.2)
            &.highLight
              background-color: rgb(0, 160, 220)
            .icon-shopping_cart
              line-height: 44px
              font-size: 24px
              color: rgba(255, 255, 255, 0.4)
              &.highLight
                color: #fff
          .num
            position: absolute
            top: 0
            right: 0
            width: 24px
            height: 16px
            line-height: 16px
            text-align: center
            background-color: rgb(240, 20, 20)
            font-size: 10px
            font-weight: 700
            color: #fff
            -webkit-border-radius: 16px
            -moz-border-radius: 16px
            border-radius: 16px
            -webkit-box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.4)
            -moz-box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.4)
            box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.4)
        .price
          display: inline-block
          vertical-align: top
          margin-top: 12px
          padding: 0 12px 0 0
          font-size: 16px
          color: rgba(255, 255, 255, 0.4)
          font-weight: 700
          line-height: 24px
          border-right: 1px solid rgba(255, 255, 255, 0.1)
          &.highLight
            color: #fff
        .desc
          display: inline-block
          vertical-align: top
          margin: 12px 0 0 12px
          line-height: 24px
          font-size: 12px
          color: rgba(255, 255, 255, 0.4)
      .content-right
        flex: 0 0 105px
        width: 105px
        background: rgba(255, 255, 255, 0.2)
        .pay
          font-size: 12px
          font-weight: 700
          height: 48px
          line-height: 48px
          text-align: center
          &.not-enough
            color: rgba(255, 255, 255, 0.4)
          &.enough
            background: rgb(0, 160, 220)
            color: #fff
    .ball-container
      .ball
        position: fixed
        left: 32px
        bottom: 22px
        z-index: 200
        &.drop-enter-active
          transition: all 0.4s cubic-bezier(0.49, -0.29, 0.75, 0.41)
          .inner
            width: 16px
            height: 16px
            border-radius: 50%
            background: rgb(0, 160, 220)
            transition: all 0.4s linear
    .shopCart-list
      position: fixed
      left: 0
      bottom: 48px
      width: 100%
      z-index: 30
      &.fold-enter-active, &.fold-leave-active
        transition: all 0.2s linear
        transform: translate3d(0, 0, 0)
      &.fold-enter, &.fold-leave-to
        transform: translate3d(0, 100%, 0)
      .list-header
        height: 40px
        line-height: 40px
        padding: 0 18px
        border-bottom: 1px solid rgba(7, 17, 27, 0.1)
        background-color: #f3f5f7
        .title
          float: left
          font-size: 14px
          color: rgb(7, 17, 27)
        .empty
          float: right
          font-size: 12px
          color: rgb(0, 160, 220)
      .list-content
        max-height: 215px
        padding: 0 18px
        background: #fff
        overflow: hidden
        .food
          position: relative
          height: 48px
          border-1px(rgba(7, 17, 27, 0.1))
          .name
            padding: 12px 0
            line-height: 48px
            font-size: 14px
            color: grb(7, 17, 27)
          .price
            position: absolute
            right: 96px
            bottom: 12px
            line-height: 24px
            font-size: 14px
            font-weight: 700
            color: rgb(240, 20, 20)
          .cartControl-wrapper
            position: absolute
            right: 0
            bottom: 6px
    .list-mask
      position: fixed
      width: 100%
      height: 100%
      left: 0
      top: 0
      background: rgba(7, 17, 27, 0.6)
      backdrop-filter: blur(10px)
      z-index: 20
      &.fade-enter-active, &.fade-leave-active
        transition: all 0.2s
        opacity: 1
      &.fade-enter, &.fade-leave
        opacity: 0
</style>

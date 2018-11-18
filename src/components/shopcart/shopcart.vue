<template>
    <div class="shopcart">
      <div class="content">
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
        <div class="content-right">
          <div class="pay" :class="payClass">{{payDesc}}</div>
        </div>
      </div>
    </div>
</template>

<script>
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
      }
    }
  };
</script>

<style scoped lang="stylus">
  .shopcart
    position: fixed
    left: 0
    bottom: 0
    z-index: 50
    width: 100%
    height: 48px
    .content
      display: flex
      width: 100%
      height: 100%
      background: #000
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
</style>
